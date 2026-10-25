# DR26-10-16-HUB-08-2027 — raw report from grok

> Unedited output of one model. Our reconciled synthesis is the page one level up.

I'll inspect upstream source for Screenpipe's audio path and the capture, ASR, and diarization alternatives, then write the comparison from those pages.The first pass found the Screenpipe audio crate and several dual-capture apps. Next I’ll open the flags, capture loop, and disk-stop code rather than the READMEs.A. Executive summary

Research date: 2026-10-17. Source of record for Screenpipe is `main` at `5ce7edfcda145fa1bb074574925b290a1356bf2c`, committed 2026-10-16T23:35:10Z. The newest published desktop tag found is `app-v2.7.109` at `4162e6a`, 2026-10-14. Those two are not the same commit. Code lines below were read from `5ce7edfc`, not diffed against the tag. [established]

Screenpipe does have a real audio path that can start while the vision pipeline is not started. `--disable-vision` skips monitor enumeration and does not construct `VisionManager`. `--disable-audio` is a separate flag. Default chunk length is 30 seconds. Default behavior follows the system default audio devices. Windows system audio in this tree is WASAPI, including endpoint loopback and, on Windows build 20348 or newer, per-process loopback. Captured audio is later encoded to AAC in an MP4 at 64 kbps. [established]

That is not the same as a proven audio-only product. Audio and vision still share one engine process, one data directory, one shutdown path, a power manager, and a schedule monitor. Low-battery and meeting-only modes can pause audio by themselves. A screen-permission denial is wired as a vision requirement only when vision is enabled, so it should not be the thing that asks for screen capture after `--disable-vision`. Whether a disk-full condition stops capture while `/health` stays green was not found in the current source. Issue #1162, the request to refuse start on low disk, was closed on 2026-04-14 as stale. A code index search for disk-full strings in this repository returned no hits. An index miss is not proof the check is absent. [established for the flags; unknown for the disk incident]

No maintained open-source application inspected here is a 24/7 mic-plus-loopback recorder with a durable queue, Russian/English transcription, and anonymous diarization. Meetily v0.4.1 (published 2026-09-12, MIT community) and Anarlog (`e8a9dec`, 2026-10-16, MIT community layer) are meeting notetakers. Hearsay is a manual tray recorder. `omni-meeting-recorder` last moved on 2026-01-31 and writes MP3 only. sososo sends audio to Deepgram and is AGPL-3.0 for the community edition. [established]

The repairable design is a small OS-specific capture adapter, immutable chunks on a non-OS disk, a durable queue, VAD, then ASR, then diarization of the loopback channel only. Do not mix microphone and loopback before diarization. pyannote community-1 averages multichannel audio down to mono. Whisper large-v3 remains the widest mixed-language model and is MIT, and it is the model with a published silence-hallucination failure. GigaAM-v3 is MIT and Russian-specialized. Parakeet TDT 0.6B v3 is CC-BY-4.0 and lists both Russian and English among 25 languages. None of those cards supplies a measured Russian-English code-switch WER. [established architecture; unknown code-switch accuracy]

Recommendation: do not retune the live Screenpipe install. Build option C in a sandbox. The main assumption is that the required product is a continuous two-sided audio archive, and that summaries, commitments, and vault ingestion stay optional until Anton says otherwise. Confidence in that recommendation: high for the boundary, medium for the model pair, because code-switch accuracy is unmeasured.

B. Findings

B1. What "audio-only Screenpipe" actually does

The CLI struct `RecordArgs` defines the gates. Permalink lines are from `crates/screenpipe-engine/src/cli/mod.rs` at `5ce7edfc`:

- `audio_chunk_duration` defaults to 30. https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-engine/src/cli/mod.rs#L458-L460
- `disable_audio` defaults to false. Lines 466-468 of the same file.
- `use_system_default_audio` defaults to true. Lines 474-476.
- `disable_vision` is documented in the flag comment as disabling the full vision pipeline, "screen images + accessibility/OCR". Lines 615-618.
- `disable_screenshots` is a different flag. It disables screenshot pixels, JPEG, and OCR while keeping accessibility-tree capture. Lines 620-622.

In `screenpipe-engine.rs`, vision and audio are branched separately:

- Monitors are enumerated only when `disable_vision` is false, specifically to avoid the macOS "Currently Sharing" indicator. https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-engine/src/bin/screenpipe-engine.rs#L1032-L1039
- Audio devices are resolved only when `disable_audio` is false. Lines 1041-1050. An empty device list logs a warning. It does not, in the lines read, exit.
- `VisionManager` is constructed only inside `if !config.disable_vision`. The else branch spawns `std::future::pending` so the server select does not treat a finished vision task as shutdown. Lines 1365-1447.
- Audio start is later and independent: `if !config.disable_audio` sleeps 10 seconds, then calls `audio_manager.start()`. Lines 1975-1982.
- macOS preflight sets `need_screen = !config.disable_vision` and `need_audio = !config.disable_audio`. Lines 914-915. With vision disabled, the screen prompt is not the required permission on that path.
- ffmpeg is required only when audio is enabled. The comment says vision-only mode must not try a network install. Lines 879-882.

So disabling vision, in this source, does not start `VisionManager`, does not list monitors, and does not require the macOS screen permission. Screenshot capture, OCR, and the accessibility tree are inside that vision pipeline according to the flag comment. HD video is tied to vision plus `hd_recording_default == Always` in the media-required check at lines 872-877. A separate encoder started outside `VisionManager` was not found in the lines read. [source-proven for the branches read; unknown for every possible screenshot call site outside those branches]

Desktop settings were not line-traced into the Tauri store in this run. The CLI names above are the verified names. A 2026-07-22 Japanese settings walkthrough describes separate audio and screen switches. That page is a secondary description, not the settings component. https://note.com/curiosity_com/n/nf97f462a36f6 [low-authority]

B2. Capture, flush, persistence, transcription

Windows far-end audio is implemented in `crates/screenpipe-audio/src/core/process_tap/windows.rs`. The file header states:

- Endpoint loopback exists on Windows 7 and later. Event-driven endpoint loopback is described as reliable on Windows 10 1703 and later.
- Per-PID process loopback requires build 20348 or Windows 11, using `AUDIOCLIENT_ACTIVATION_TYPE_PROCESS_LOOPBACK`.
- Below that build, the code refuses to widen a meeting tap into the whole system mix. Full-endpoint loopback remains available through a separate call.

https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-audio/src/core/process_tap/windows.rs#L5-L25

Microsoft's own loopback document matches the API shape: open the render endpoint, initialize capture with `AUDCLNT_STREAMFLAGS_LOOPBACK`, shared mode only. Event-driven loopback without a render-stream workaround needs Windows 10 1703 or later. Protected DRM audio is not captured by a trusted driver. Last updated 2025-04-16. https://learn.microsoft.com/en-us/windows/win32/coreaudio/loopback-recording [established]

The chunk collector in `run_record_and_transcribe.rs` keeps microphone and speaker receivers separate, resamples, and can run an AEC path. `flush_audio` sends float samples to the transcription channel and keeps an overlap tail. It is not, in the function read, a disk fsync. https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-audio/src/core/run_record_and_transcribe.rs#L809-L819

Disk format is ffmpeg AAC in MP4, 64 kbps, `aac_low`, `+faststart`. The path is `{device}_{YYYY-MM-DD_HH-MM-SS}.mp4`. A unique path is reserved with `OpenOptions::create_new`. On encode failure the reserved file is removed. https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-audio/src/utils/ffmpeg.rs#L72-L86 and lines 216-229. [source-proven]

Arithmetic from that bitrate, not a measured directory size: 64 kbps is 8 KB/s, about 0.66 GiB per continuous stream per day. Two always-on streams are about 1.3 GiB/day if silence is encoded too. Whether silent chunks are dropped before the MP4 write was not traced. Uncompressed 16 kHz mono s16 would be about 2.57 GiB/day per channel. [derived, not measured]

`data_dir` exists and defaults to `$HOME/.screenpipe`. https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-engine/src/cli/mod.rs#L504-L506 Issue #2458 (opened 2026-03-11) reported that the disk-usage UI ignored a custom data directory. The maintainer closed it on 2026-03-16 as fixed in recent builds. https://github.com/screenpipe/screenpipe/issues/2458 [single-source for the bug, maintainer close note]

Transcription is downstream of the audio manager. Release `app-v2.7.109` (2026-10-14) includes "transcribe short speech in long chunks by measuring speech ratio over a 5 s window (SCR-583)". https://newreleases.io/project/github/screenpipe/screenpipe/release/app-v2.7.109 The engine still inserts `vad_engine = Silero` into its runtime map. https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-engine/src/bin/screenpipe-engine.rs#L730 A file named `e2e_ghost_word_silent_room.rs` exists under `crates/screenpipe-audio/src/core/`. Its body was not read. The name is evidence the project tests ghost words on silence. It is not a published WER. [source-proven that the test file exists]

Speaker handling is a separate stage. `start_speaker_identification` is imported by the engine binary. Historical bug: Screenpipe 0.3.243 on 2026-04-08 still downloaded `wespeaker_en_voxceleb_CAM++.onnx` when launched with `--disable-audio`, and a 120-second timeout exited the process before health. https://github.com/screenpipe/screenpipe/issues/2859 That is speaker-embedding identification, which is a different claim from anonymous diarization. Whether 5ce7edfc still downloads that model with audio disabled was not re-read. [historical, version-pinned]

Search and export exist as engine routes (`routes/audio.rs` lists devices and posts device-status events) and as a CLI export command. A hybrid activity-summary test file exists (`activity_summary_hybrid_tests.rs`). A Parquet writer was not opened. The local claim that a hybrid Parquet index shows misleading text is unverified. [unknown]

B3. What else can stop audio while vision is off

These are independent of the vision flag:

- Power profiles. PR #3544 (2026-05-23) describes audio pause at or below 20 percent battery and full capture pause at or below 10 percent or OS low-power mode. The server is described as staying up. https://github.com/screenpipe/screenpipe/pull/3544 Merge at `5ce7edfc` was not re-checked line by line. The engine does call `start_power_manager`. [documented in the PR; current wiring partially proven by the call]
- Schedule. PR #2952 (merged 2026-04-13) says the schedule previously saved rules without stopping capture, and the fix stops and restarts `VisionManager` and `AudioManager` together. https://github.com/screenpipe/screenpipe/pull/2952
- Meeting-only mode. PR #3559 (2026-05-23) adds `--audio-meetings-only`, default off. It gates persist and STT. The PR says CPAL streams stay running. https://github.com/screenpipe/screenpipe/pull/3559
- Default-device enrollment. PR #3656 (2026-05-27) describes `useSystemDefaultAudio`, tray mute stored as `user_disabled_devices`, and recovery when a stream is disconnected or zero-filled. `windows_output_follow.rs` is a 39,034-byte file on `main`. Its state machine was not read. [documented design; file present; behavior not line-audited]
- Microphone denial is logged as its own Windows pattern, "Access is denied", in the Sentry filter. That filter classifies it as environment, not an engine bug. It does not show that vision is required for the failure. https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-engine/src/bin/screenpipe-engine.rs#L624-L631
- Screen-permission issue #2732 (2026-04-05) says the error is "Screen recording permission denied. Vision capture is disabled." It does not say the audio worker is stopped. https://github.com/screenpipe/screenpipe/issues/2732
- Sleep and wake: `start_sleep_monitor` is called. The body was not read. [unknown]
- Process crash: one OS process hosts both. A panic that aborts the process stops audio even if vision is disabled. Task-level isolation was not proven.
- Older macOS reconnect PR #1797 described a 10-attempt, 5-second cap after 48-hour drops. That text is a PR description, not current Windows code. [historical, not current-Windows]

B4. License boundary for Screenpipe

`LICENSE.md` on `main` is the Screenpipe Commercial License, copyright 2024-2026 Negentropy Labs, Inc. Free use covers personal non-commercial use, nonprofit, educational, or research use, and evaluation for up to seven days. Commercial use, defined as business or production use, revenue support, or use by a for-profit entity after evaluation, needs a paid license. Section 6 says the licensor retains rights in the work, including modifications or patches the user makes. Official prebuilt binaries are governed by the Terms of Service, not by this source license. The same file says older MIT-tagged versions remain MIT. The license file history shows the move off MIT on 2026-06-10, commit `81e412f`. https://github.com/screenpipe/screenpipe/blob/main/LICENSE.md and https://github.com/screenpipe/screenpipe/commits/main/LICENSE.md [established text; not legal advice]

A product target in `docs/VISION.md` says under 20 percent CPU and under 3 GB RAM on release builds. That is a target for the whole screen product, not a measurement of audio-only. https://github.com/screenpipe/screenpipe/blob/main/docs/VISION.md [documented target, not measured]

B5. Alternatives that actually record both sides

Category split matters. Meeting bots and file transcribers do not answer the capture question.

| Product | Pin | Both sides on Windows | 24/7 archive | Raw audio out | License read | Verdict |
| --- | --- | --- | --- | --- | --- | --- |
| Screenpipe | main `5ce7edfc` 2026-10-16; app `app-v2.7.109` `4162e6a` 2026-10-14 | WASAPI endpoint and process loopback in source | Designed as continuous desktop capture | AAC 64 kbps MP4, not the original PCM as the stored original | Commercial license, not OSI | Usable only inside its license and still coupled to one supervisor |
| Meetily | tag `v0.4.1`, published 2026-09-12, Windows `meetily_0.4.1_x64-setup.exe` | README says mic and system audio together | Meeting session, not an always-on queue | Not traced to an immutable WAV original | README says MIT | Meeting product. Capture source was not line-read |
| Anarlog | `e8a9dec326280fe1167ab1bf09397208e5622aa0`, 2026-10-16 | WASAPI loopback in `crates/audio-actual/src/speaker/windows.rs`, from a code index snippet | Meeting memo workflow | Local meeting files; not a chunk queue | `LICENSING.md`: MIT outside `enterprise/` | Best maintained meeting app. Windows called beta by its own article. macOS loopback is not built in |
| Hearsay | `parkscloud/Hearsay`; `main` commit API returned 422 | README: WASAPI via PyAudioWPatch plus microphone via sounddevice | Tray Start/Stop, then one markdown file | Not a durable queue | License file not opened | Manual session tool. Maintenance not established |
| omni-meeting-recorder | `0fae8a3`, 2026-01-31 | README: WASAPI loopback plus mic, no virtual cable | A recording command, not a daemon | MP3 | License file not opened | Windows-only capture tool. No ASR. Nine months idle |
| sososo | README fetched via search | WASAPI on Windows; macOS needs BlackHole | Live captions | Streams audio to Deepgram | AGPL-3.0 community, plus a commercial license | Excluded. Audio leaves the machine. Copyleft if distributed |

Anarlog's own docs, updated 2026-08-30, say it records microphone and computer audio without joining as a bot, and that on-device speech can be Soniqo, Apple Speech, a whisper.cpp file, or a Deepgram-compatible local server. https://docs.anarlog.so/ Their licensing split is path-based MIT versus commercial `enterprise/`. https://github.com/fastrepl/anarlog/blob/main/LICENSING.md A 2026-06-03 secondary article says Windows and Linux builds exist and that Apple Silicon is the most tested path. https://runany.dev/blog/anarlog-open-source-ai-meeting-notetaker/ [article is single-source; license file and commit pin were opened]

Meetily README states simultaneous mic and system audio, Vulkan Whisper on the packaged Windows build, and MIT. https://github.com/Zackriya-Solutions/meetily/blob/main/README.md Release API: tag `v0.4.1`, published 2026-09-12T06:15:42Z. https://github.com/Zackriya-Solutions/meetily/releases/tag/v0.4.1 The tag SHA was not in the API payload. Speaker identification is described by a third-party comparison as a Pro feature, and by a fork README as something that fork unlocks. Those are secondary. The community README opened here does not itself prove anonymous diarization in the MIT build. [partial]

PyAudioWPatch is the Windows loopback library Hearsay names. Releases page lists `v0.2.12.9` as Latest, commit `8b68e78`. The extracted date string was "06 Oct" without a year. https://github.com/s0d3s/PyAudioWPatch/releases Loopback devices are exposed as extra input devices. https://github.com/s0d3s/PyAudioWPatch The Rust `wasapi` crate documents loopback plus notifications for device add, remove, and default-device change, MIT, and states Rust 1.85. Page fetched with a 2026-10-11 publication stamp on the index. https://lib.rs/crates/wasapi [library capability established; 24/7 behavior not measured]

Current FFmpeg device documentation, fetched 2026-10-17, has no "wasapi" or "loopback" string in the 2,037 lines returned. https://ffmpeg.org/ffmpeg-devices.html So ffmpeg is established as Screenpipe's encoder, and not established from that manual as the Windows loopback capture device. macOS AVFoundation is documented there, including a `none` token so one side can be omitted. That is file-device capture, not a durable queue.

Windrecorder's README describes screen, OCR, and video maintenance. It was not established as a mic-plus-loopback transcript pipeline. https://github.com/yuka-friends/Windrecorder [screen product, out of category]

B6. ASR for Russian and English

These are separate numbers. A leaderboard WER is not diarization error, not code-switch error, and not silence behavior.

Whisper, official README, MIT for code and weights. Paper: https://arxiv.org/abs/2212.04356 Relative speed on an A100 for English: tiny about 10x, large 1x and about 10 GB VRAM, turbo about 8x and about 6 GB VRAM, 809 M parameters. Turbo is an optimized large-v3 and is not the translation model. The CLI transcribes a file with a sliding 30-second window. https://github.com/openai/whisper/blob/main/README.md [established for those VRAM and speed notes; hardware is an A100, not this Windows host]

Silence hallucination is a published failure of Whisper, not a rumor. Koenecke and colleagues, submitted 2024-02-12, revised 2024-05-03, FAccT 2024: about 1 percent of transcriptions contained a fully hallucinated phrase, and hallucinations were disproportionate for speakers with longer non-speech durations. https://arxiv.org/abs/2402.08021 WhisperX, submitted 2023-03-01, revised 2023-07-11, states that long-audio sliding windows drift, hallucinate, and repeat, and that VAD cut-and-merge plus forced alignment is the remedy. https://arxiv.org/abs/2303.00747 [established]

faster-whisper is the backend WhisperX documents for batched inference. The PyPI text says batched large-v2 at about 70x realtime and under 8 GB GPU memory for large-v2 with beam size 5. https://pypi.org/project/whisperx/ The faster-whisper license file was not opened in this run. WhisperX's own license is described as BSD-2-Clause on secondary pages and on PyPI packaging. Treat redistribution terms as unread until the LICENSE file is opened. [partial]

GigaAM-v3, Hugging Face card `ai-sage/GigaAM-v3`, license MIT, 220-240 M parameters, Russian ASR, paper https://arxiv.org/abs/2506.01192 (InterSpeech 2025). Revisions include `e2e_rnnt`, which the README says adds punctuation and text normalization. The GitHub README dates v3 to 2025-11 and says pretraining used 700,000 hours and ASR training 4,000 hours. A separate multilingual line is listed at 2,000,000 pretrain hours and 50,000 ASR hours. https://huggingface.co/ai-sage/GigaAM-v3 and https://github.com/salute-developers/GigaAM [established for license, size, and the hours the project itself states]

Public WER claims for GigaAM disagree with each other because the sets differ. A Habr post of 2026-02-21 reports 3.3 percent WER for GigaAM-v3 RNNT on that author's set. https://habr.com/ru/articles/1002260 A Habr post of 2026-06-02 reports about 10.38 percent aggregate WER for the same model family on six Russian sets, close to a fine-tuned Whisper turbo at 9.43 percent. https://habr.com/ru/articles/1042574 A Habr post of 2026-10-05 on sung Russian reports roughly 18 percent macro WER for GigaAM-v3 and 17.7 percent for WhisperX large-v3 after vocal separation, and worse numbers for Parakeet, Qwen3-ASR, and Canary on a five-song slice. https://habr.com/ru/articles/1090474 Those are practitioner reports. They are not one benchmark. An X post on 2026-10-08 attributes a RESON OSD Russian result to MWS AI (7.4 errors per 100 words for GigaAM-v3, 15.8 Parakeet, 25.5 Nemotron, and a noise gap versus Whisper). The MWS report itself was not opened. [single-source, low-authority for the MWS figures]

Parakeet TDT 0.6B v3, Hugging Face release date on the card 2025-08-14, CC-BY-4.0. It lists 25 European languages, explicitly including English and Russian, auto language detection, punctuation, word and segment timestamps, 16 kHz mono input, and long audio up to 24 minutes with full attention on an A100 80 GB or up to 3 hours with local attention. https://huggingface.co/nvidia/parakeet-tdt-0.6b-v3 The WER bar chart on that card did not yield extractable numbers. The Open ASR leaderboard page did not render its table in this fetch. A secondary 2026-07-18 blog states 6.34 percent average WER and 3,332x realtime. That number is not repeated here as established. https://openwhispr.com/blog/parakeet-vs-whisper-vs-nemotron [secondary, unconfirmed in this run]

Transducer models are widely described as less prone than Whisper's decoder to inventing text on silence. That is an architectural argument plus secondary writeups, not a Russian silence corpus result opened here. [emerging]

Hosted transcription (Deepgram, OpenAI, Speechmatics, and Screenpipe Cloud) can be more accurate and cheaper on GPU, and it uploads audio. This brief forbids unapproved upload. It stays a later option, not part of the canary.

B7. Diarization is "who spoke when", not identity

pyannote `speaker-diarization-community-1` ingests mono 16 kHz. Stereo or multichannel files are downmixed by averaging channels. The card's table, last updated 2025-09, with no forgiveness collar and without skipping overlap, includes AMI IHM 17.0 percent DER versus legacy 18.8, AMI SDM 19.9, DIHARD 3 full 20.2, VoxConverse 11.2, CALLHOME part 2 26.7, MSDWild 22.8. Overlap-heavy sets are much worse: AVA-AVD 44.6, Ego4D dev 46.8. License on the card: CC-BY-4.0. Access requires accepting conditions and a Hugging Face token. https://huggingface.co/pyannote/speaker-diarization-community-1 Exclusive mode, documented by pyannote, iterates turns with overlap removed. https://www.pyannote.ai/blog/community-1 The blog page carried a 2026-10-12 fetch stamp and describes the model as the open model shipped with pyannote.audio 4.0. [established for the card table; the blog date is the fetch stamp]

NVIDIA streaming Sortformer 4-speaker v2.1 uses an arrival-order speaker cache so labels can stay consistent inside one stream. License on the card: `nvidia-open-model-license`, which is not CC-BY. https://huggingface.co/nvidia/diar_streaming_sortformer_4spk-v2.1 Nemotron 3 Diarization supports up to eight speakers, streaming and offline, arrival order, and the card says it is ready for commercial or non-commercial use under OpenMDW-1.1. https://huggingface.co/nvidia/Nemotron-3-Diarization NVIDIA's 2025-08-18 technical post, with a September 2026 note pointing at Nemotron 3, describes frame-level tags and the cache. It says the streaming model is optimized for English and was also tried on Mandarin meetings and non-English CALLHOME. https://developer.nvidia.com/blog/identify-speakers-in-meetings-calls-and-voice-apps-in-real-time-with-nvidia-streaming-sortformer/ [established for scope; Russian DER unknown]

Consequences for this pipeline:

- A label such as Speaker A is anonymous. It is not a person record. Renaming requires a later human or a separate, explicit enrollment step. Enrollment is speaker recognition and was not requested.
- pyannote and Sortformer reset or re-order speakers per audio object unless a cache or an embedding join is added. Chunked files will renumber speakers unless the join exists. Sortformer's cache is inside one stream, not across a process restart. [established for the documented cache; cross-restart behavior unknown]
- Mixing mic and loopback, then diarizing, creates echo duplicates: the same local voice can appear on both channels, often delayed. The correct split is role by channel. Local channel stays "Local". Remote channel is diarized into Speaker A, Speaker B. Overlap inside the remote mix still needs a model that allows overlap. Averaging the two channels first, which community-1 will do if handed a stereo file, destroys that split.
- Confidence should be stored. A turn under a chosen overlap or embedding threshold stays "unknown" rather than a forced name. No threshold was calibrated on this corpus. [speculative until the canary]

Normalization, if it is ever done, has to be a reversible layer. Store the raw transcript. A later view may fix punctuation. It must not change negation, amounts, or names. GigaAM's e2e revision applies its own text normalization. That output is a view, not the archive. [design requirement; model behavior stated by the GigaAM README]

B8. Monitoring

Process-up is the wrong probe. Screenpipe's historical health port is 3030, and this engine configures whether media is "required". The current health JSON fields were not read. Do not treat a green process as fresh audio. [unknown for the current payload]

A probe that matches the failure mode has two independent signals per source:

1. Capture: device present, callback age, peak or RMS of the last completed chunk, bytes committed, and the manifest hash. Legitimate silence is a fresh chunk whose peak is below a measured noise floor and whose device handle is alive. A dead path is no new chunk, a stalled callback, a removed device, or a write error.
2. Transcript: count of chunks where VAD found speech and a transcript row was committed, with model id and config hash. VAD-negative silence with an empty transcript is success. VAD-positive speech with no transcript is failure.

Negative controls: unplug the mic, change the default output, play a known marker, inject a digital-silence file, fill a small volume. Each must flip a different alarm.

C. Contradictory evidence

- Screenpipe is and is not audio-only. The engine branches prove vision can be left unstarted while audio starts. The same binary still has power, schedule, meeting, PII, and health machinery that can pause or gate audio. Both statements are true at different layers.
- "Open source Screenpipe" is true for older MIT tags and false for current `main`. The license file says both.
- GigaAM "wins" and "ties" Whisper. The 3.3 percent Habr result and the about 10 percent six-set Habr result use different audio. Sung-speech numbers favor WhisperX by a small margin after separation. No single winner is established for conversational Russian-English.
- Diarization "about 11 to 19 percent DER" appears in secondary 2026 guides. The primary community-1 table is harsher on hard sets, 44 to 47 percent DER on AVA-AVD and Ego4D. Quoting only AMI or VoxConverse overstates meeting-room difficulty and understates overlap.
- Parakeet "6.34 percent and silence-safe" is a secondary summary. The model card confirms language list, license, and architecture. It does not, in the text extracted here, confirm that WER or a silence-hallucination rate.
- ffmpeg is central to Screenpipe's audio files and is not, in the current device manual, a documented WASAPI capture device.
- Issue #1162 is closed "completed" and the closing comment says it was stale. Closure is not evidence a disk guard shipped.
- Anarlog and Meetily marketing says local. Their own texts also allow cloud summaries or cloud speech when configured. Local weights do not make a later LLM call local.

D. Pinned source-code map

Screenpipe, repository https://github.com/screenpipe/screenpipe

| Stage | File at `5ce7edfc` | What was proven |
| --- | --- | --- |
| Entrypoint | `crates/screenpipe-engine/src/bin/screenpipe-engine.rs` | Branches vision, audio, permissions, ffmpeg, and `audio_manager.start()` |
| Flags | `crates/screenpipe-engine/src/cli/mod.rs` lines 458-476 and 615-622 | `--disable-audio`, `--disable-vision`, `--disable-screenshots`, `--use-system-default-audio`, 30 s chunks, `--data-dir` |
| Windows loopback | `crates/screenpipe-audio/src/core/process_tap/windows.rs` lines 5-25 | Endpoint loopback versus per-PID loopback and the build 20348 gate |
| Chunk assembly | `crates/screenpipe-audio/src/core/run_record_and_transcribe.rs` | Separate mic and speaker receivers, overlap flush into the transcription channel |
| Persistence | `crates/screenpipe-audio/src/utils/ffmpeg.rs` lines 72-86 and 216-229 | AAC 64 kbps MP4, `create_new`, delete reserved file on encode error |
| Device follow | `crates/screenpipe-audio/src/audio_manager/windows_output_follow.rs` present, unread | File exists. Recovery behavior not audited |
| API | `crates/screenpipe-engine/src/routes/audio.rs` | Device list and device control. Not a freshness proof |
| License | `LICENSE.md` on main, history commit `81e412f` 2026-06-10 | Commercial license, MIT only for older releases |

Not read, so not claimed: the body of `device_monitor.rs` (166,910 bytes), `reconciliation.rs`, the health handler, the Tauri settings binding, the Parquet writer, and sleep-monitor behavior.

E. Options

|  | A. Screenpipe with vision off | B. Meetily or Anarlog | C. Minimal recorder plus queue plus ASR |
| --- | --- | --- | --- |
| Fits 24/7 mic and loopback | Partial. Flags exist. Supervisor still shared | No. Session meeting tools | Yes, if the adapter is written that way |
| Original audio kept | MP4/AAC, original PCM not the stored file | Not established | WAV or PCM chunk, immutable |
| Stops because vision, OCR, or screen permission failed | Screen permission is not required once vision is off. Shared process crash still stops audio | Not applicable | Capture process has no OCR |
| Disk pressure behavior | Not found as a designed threshold. OS write failure still possible | Unknown | Explicit probe, refuse new chunks, delete nothing |
| Russian plus English | Inherits the selected Whisper or cloud engine. Cloud uploads audio | Whisper or Parakeet, meeting length | GigaAM and Parakeet or Whisper behind VAD, chosen per segment |
| Diarization | Speaker identification models exist. Anonymous cross-chunk diarization not proven | Community versus Pro is disputed in secondary sources | Loopback channel only, Speaker A/B |
| License for a for-profit production install | Paid commercial license after 7 days. Modifications claimed by the licensor | Meetily MIT. Anarlog MIT outside enterprise | Permissive pieces if CC-BY attribution is kept and NVIDIA custom licenses are read before redistribution |
| macOS | Same product, different capture (ScreenCaptureKit in the doctor text) | Anarlog macOS is the mature build. Windows is beta in their article | Separate adapter. Not the same code path |
| CPU, RAM, GPU, disk, latency | Unknown for audio-only. Product target under 20 percent CPU and 3 GB is for the whole app | Unknown on this host | Unknown until the canary. Encoder math above is not a measurement |

F. Failure modes

1. Vision disabled, audio still dies because the shared process panics, the schedule pauses both, battery policy pauses audio, or meeting-only mode is on.
2. Default output changes. Endpoint loopback follows the device it opened, not "whatever is default now", unless a follower reopens it. Screenpipe has a follower file. A naive WASAPI open does not.
3. Bluetooth and USB headsets renegotiate format. Capture must reopen. Exclusive-mode clients cannot loopback, per Microsoft.
4. DRM playback is silence on the loopback pin even though the user hears it.
5. Per-process loopback on Windows 10 older than build 20348 is unsupported in Screenpipe's meeting tap. Using it by accident records nothing. Using full-endpoint loopback records the whole mix, including local TTS and notification sounds.
6. Whisper on silence writes fluent false text. VAD-before-ASR is the published mitigation. VAD can also drop quiet speech. The 2026-10-14 Screenpipe fix is about short speech inside a long chunk being thrown away by a speech-ratio gate. That is the opposite failure.
7. One stereo file into pyannote averages the channels and glues echo into fake speakers.
8. Chunked diarization renumbers Speaker A every 30 seconds.
9. GigaAM e2e normalization rewrites numbers. If that string replaces the raw transcript, amounts are no longer evidence.
10. Health stays green because the process is up and the last error was swallowed. Disk full, stuck device, and all-zero callbacks look alike without a sample probe.
11. `create_new` plus delete-on-encode-failure means a failed encode leaves no audio. That is the opposite of an immutable original.
12. Commercial-license Screenpipe on a for-profit machine after seven days is a license failure even if the audio works.
13. Code-switch inside one breath is the unmeasured case. Picking the Russian specialist for an English clause, or Whisper for a noisy Russian name, both fail differently.
14. Abandoned pattern: one "record everything" app that also OCRs the screen, then hoping the audio half is reliable. The source shows the flags. It does not show an isolated audio supervisor.

G. Recommendation

Choose C. Leave the live Screenpipe configuration unchanged.

Assumption: the job is a continuous archive of the microphone and of what the speakers played, with timestamps, on a data disk, transcribed for Russian and English, with anonymous speaker turns. Summaries and vault notes are not in scope. Assumption two: a for-profit production install should not depend on the current Screenpipe commercial license. If the use is strictly personal and non-commercial, the license objection shrinks, and the supervisor-coupling objection remains.

Trade-off accepted: C is more code than a checkbox, and less code than adopting Screenpipe or a meeting suite and then fighting their product boundaries. Meetily and Anarlog are the right tools for a meeting note, and the wrong tools for a 24/7 archive. Screenpipe is the right tool for screen memory, and a coupled tool for audio-only reliability.

Model pair, medium confidence: Silero VAD or an equivalent energy-plus-VAD gate first. Russian segments to GigaAM-v3 e2e RNNT, raw text retained beside any normalized text. English and other listed European languages to Parakeet TDT 0.6B v3. If a segment is mixed inside a few seconds, send that segment to Whisper large-v3 or turbo and keep the other models' text as a second hypothesis until a code-switch set exists. Do not upload audio. Diarize only the loopback channel, with pyannote community-1 for offline chunks or Nemotron 3 if the OpenMDW terms are acceptable after a direct read of the license deed. Labels stay Speaker A and Speaker B. Join labels across chunks with embeddings, and drop the join on low similarity instead of guessing.

Confidence: high that vision-off Screenpipe is not a sufficient reliability design. Medium that this model pair is the best local default. Low on exact CPU and WER until the canary.

H. Staged action plan

No fleet rollout. No change to the running recorder. Data directory on a volume that is not the OS drive.

Before: Screenpipe, if it is running, keeps doing what it does now. No new archive, no new deletion rule.

After phase 1: a sandbox process writes 30-second WAV chunks for mic and for loopback, plus a JSON manifest. Fields: UTC start, UTC end, device id, role (`mic` or `loopback`), channels, sample rate, format, sha256, hostname. Write to a temporary name, fsync, then replace. A failed replace leaves the previous good chunk. Nothing is deleted to free space. If free space falls below a named floor, capture of new chunks stops and the manifest records `stopped_disk`. The process exit is nonzero. Rollback is to stop that process. Existing chunks stay.

Acceptance for phase 1: 24 hours in a sandbox with a speech marker every hour, a five-minute silence, a device unplug and replug, a default-output change, a process kill and restart, and a tiny volume filled to the floor. Pass means each mic and loopback hour has the expected chunk count except during the induced outage, the outage is labeled, silence chunks exist and are low-energy, and no chunk was deleted. CPU, RAM, and bytes per hour are written down from that run. They are unknown today.

Phase 2: a second process reads the manifest, skips hashes already transcribed, runs VAD, and writes a transcript row beside the chunk. Idempotent means the same hash does not call the model twice. Backpressure means a full queue pauses capture only by a visible alarm, or spills manifests, and does not drop the WAV. Rollback is to stop the worker. Audio chunks remain valid.

Acceptance for phase 2: a planted Russian utterance, a planted English utterance, a silence file, and an overlap clip. Silence must produce no words. The planted clips must contain the planted names and numbers. WER and DER are computed only on those planted references, and the report must say the set is a canary, not a population estimate. A missing reference means the metric is not reported.

Phase 3: diarization on loopback chunks only, with a cross-chunk embedding map stored separately from the raw transcript. Low similarity starts a new anonymous label. Echo of the local voice on the loopback channel is marked duplicate, not a new person.

Phase 4, optional, blocked on a user confirmation: dedupe, a normalization view that cannot edit the raw row, summaries, commitment extraction, and a source-linked vault note. Not part of "done" for this research.

I. Sources

Opened in this run, with the date they carry:

- https://github.com/screenpipe/screenpipe/commit/5ce7edfcda145fa1bb074574925b290a1356bf2c 2026-10-16
- https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-engine/src/cli/mod.rs
- https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-engine/src/bin/screenpipe-engine.rs
- https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-audio/src/core/process_tap/windows.rs
- https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-audio/src/core/run_record_and_transcribe.rs
- https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-audio/src/utils/ffmpeg.rs
- https://github.com/screenpipe/screenpipe/blob/5ce7edfcda145fa1bb074574925b290a1356bf2c/crates/screenpipe-engine/src/routes/audio.rs
- https://github.com/screenpipe/screenpipe/blob/main/LICENSE.md and https://github.com/screenpipe/screenpipe/commits/main/LICENSE.md 2026-06-10 and 2026-10-14 edits
- https://github.com/screenpipe/screenpipe/blob/main/docs/VISION.md
- https://github.com/screenpipe/screenpipe/tags `app-v2.7.109` 2026-10-14
- https://newreleases.io/project/github/screenpipe/screenpipe/release/app-v2.7.109 2026-10-14
- https://github.com/screenpipe/screenpipe/issues/1162 closed 2026-04-14
- https://github.com/screenpipe/screenpipe/issues/2458 2026-03-11
- https://github.com/screenpipe/screenpipe/issues/2732 2026-04-05
- https://github.com/screenpipe/screenpipe/issues/2859 2026-04-08
- https://github.com/screenpipe/screenpipe/issues/2115 2026-01-30 WebRTC VAD frame-size failure on Windows, historical
- https://github.com/screenpipe/screenpipe/pull/2952 2026-04-13
- https://github.com/screenpipe/screenpipe/pull/3544 2026-05-23
- https://github.com/screenpipe/screenpipe/pull/3559 2026-05-23
- https://github.com/screenpipe/screenpipe/pull/3656 2026-05-27
- https://learn.microsoft.com/en-us/windows/win32/coreaudio/loopback-recording updated 2025-04-16
- https://ffmpeg.org/ffmpeg-devices.html fetched 2026-10-17, no WASAPI entry
- https://github.com/openai/whisper/blob/main/README.md
- https://arxiv.org/abs/2212.04356 Whisper paper, referenced by that README
- https://arxiv.org/abs/2402.08021 2024-02-12, revised 2024-05-03
- https://arxiv.org/abs/2303.00747 2023-03-01, revised 2023-07-11
- https://pypi.org/project/whisperx/ page stamp 2026-06-26
- https://huggingface.co/pyannote/speaker-diarization-community-1 table updated 2025-09
- https://www.pyannote.ai/blog/community-1
- https://huggingface.co/nvidia/parakeet-tdt-0.6b-v3 card release 2025-08-14
- https://huggingface.co/nvidia/diar_streaming_sortformer_4spk-v2.1
- https://huggingface.co/nvidia/Nemotron-3-Diarization
- https://developer.nvidia.com/blog/identify-speakers-in-meetings-calls-and-voice-apps-in-real-time-with-nvidia-streaming-sortformer/ 2025-08-18
- https://huggingface.co/ai-sage/GigaAM-v3
- https://github.com/salute-developers/GigaAM
- https://arxiv.org/abs/2506.01192
- https://github.com/Zackriya-Solutions/meetily/blob/main/README.md
- https://github.com/Zackriya-Solutions/meetily/releases/tag/v0.4.1 2026-09-12
- https://github.com/fastrepl/anarlog/commit/e8a9dec326280fe1167ab1bf09397208e5622aa0 2026-10-16
- https://github.com/fastrepl/anarlog/blob/main/LICENSING.md
- https://docs.anarlog.so/ 2026-08-30
- https://github.com/dobachi/omni-meeting-recorder/commit/0fae8a3c68c3a0775e89e5666fc01c177db5d799 2026-01-31
- https://github.com/s0d3s/PyAudioWPatch/releases
- https://lib.rs/crates/wasapi
- https://habr.com/ru/articles/1002260 2026-02-21
- https://habr.com/ru/articles/1042574 2026-06-02
- https://habr.com/ru/articles/1090474 2026-10-05

Secondary, used only with that label: https://note.com/curiosity_com/n/nf97f462a36f6 2026-07-22, https://openwhispr.com/blog/parakeet-vs-whisper-vs-nemotron 2026-07-18, https://runany.dev/blog/anarlog-open-source-ai-meeting-notetaker/ 2026-06-03, the 2026-10-08 X post on RESON OSD, and https://www.pyannote.ai/blog/pyannote-vs.-nvidia-nemo-diarization-accuracy-setup-and-production-readiness 2026-10-12, which is a vendor comparison.

J. Explicit unknowns

- Current `/health` JSON, and whether it can be green with zero new samples.
- Whether silent chunks are still written to MP4.
- Line-level behavior of `windows_output_follow.rs`, sleep/wake, and panic isolation.
- Tauri checkbox to CLI flag mapping on the installed app build, as opposed to `main`.
- Diff between `app-v2.7.109` and `5ce7edfc` for these paths.
- Any disk-threshold code whose strings were missed by the code index.
- Parquet or hybrid-index text the local notes describe.
- Measured CPU, RAM, GPU, and latency of audio-only Screenpipe and of option C on this host.
- Russian-English code-switch WER for GigaAM, Parakeet, and Whisper on conversational audio.
- Russian DER for community-1 and Nemotron 3.
- Cross-restart speaker-label stability. No implementation of a durable anonymous map was found in the products reviewed.
- faster-whisper, Hearsay, and omni-meeting-recorder license files were not opened.
- OpenMDW-1.1 and the NVIDIA open model license deeds were not read beyond the model-card license name. Do not redistribute those weights until the deed is read.
- The MWS RESON OSD report was not opened. Its numbers stay unverified.
- ffmpeg WASAPI capture may exist in a wiki or an older build. It is absent from the current device manual fetched here.

The experiment that settles the remaining product doubt is the phase-1 canary, then a 30-minute planted RU/EN/silence/overlap set for WER and DER. Until those references exist, any accuracy percentage would be invented.

Practical synthesis, Russian

Живой Screenpipe не перенастраивать. Галочка «выключить экран» в текущем исходнике действительно не запускает захват экрана и OCR, но звук всё равно живёт в том же процессе, с питанием, расписанием и общим диском. Отдельного доказательства, что при забитом диске звук молча умирает, а здоровье остаётся зелёным, в исходнике не нашлось. Готовые «заметочники встреч» пишут сессию, а не круглосуточный архив, и часть из них уносит звук в облако.

Рабочая схема проще. Два независимых потока, микрофон и то, что играет в колонках, кусками по 30 секунд на отдельный диск. Сначала файл и манифест, потом распознавание, потом «кто когда говорил» только на потоке колонок. Свои слова не смешивать с чужими до этого шага, иначе эхо становится вторым человеком. Русский кусок отдавать GigaAM, английский Parakeet, спорный смешанный кусок Whisper, сырой текст не переписывать. Имена людей не угадывать, только «местный» и «говорящий A/B». Сутки прогона в песочнице с метками, тишиной, выдёргиванием устройства и заполненным маленьким диском. Пока нет эталонной расшифровки, проценты ошибок не объявлять. Сводки, обещания и записи в заметки не делать, пока это отдельно не подтверждено.

Простыми словами

Screenpipe умеет писать звук без экрана, но это всё ещё одна программа с общими выключателями. Надёжнее завести отдельный магнитофон. Он кладёт куски звука на другой диск и ничего сам не стирает. Текст делается вторым шагом. Кто говорит, подписывается как «человек A», без настоящих имён. Сначала сутки проверки на копии, боевую запись не трогать.

Coverage note, inside this report: Screenpipe `device_monitor.rs`, the health handler, the desktop settings store, and the Parquet path were not line-read. Numeric WER from the Open ASR leaderboard was not visible in the fetch. Those gaps are listed in J and are not filled with estimates.
