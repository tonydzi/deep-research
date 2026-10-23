# DR26-07-16-HUB-04 — raw report from browsers

> Unedited output of one model. Our reconciled synthesis is the page one level up.

# DR26-07-16-HUB-01 Firefox and other browsers for working around Chrome automation constraints

## Executive summary

- **Use Firefox as the default “cookie-export browser” for your fleet.** For your exact pain point—external session/cookie extraction from a real, logged-in desktop browser on Windows 11—Firefox is the cleanest fit because Mozilla’s profile model is file-oriented, geckodriver explicitly supports custom prepared profiles, and Mozilla documents both multi-profile management and container isolation. Chrome and Edge deliberately moved the opposite direction on Windows with App-Bound Encryption. **[established]** citeturn6search16turn10search1turn37view3turn37view4turn37view8
- **Do not standardize on “bypassing” Chrome ABE as an architecture.** Public research shows that user-mode bypasses exist via COM/elevation-service abuse, process hollowing, and related owner-process tricks, and some projects claim support through very recent Chromium generations. But those paths are brittle, operationally ugly, likely to trip security tooling, and are the opposite of “AK-47 maintainable.” **[emerging]** for existence of bypasses; **[established]** that Chrome hardened against exactly this class of theft. citeturn6search16turn37view1turn6search11turn6search14turn7search1turn6search17
- **For Chromium-only sites or workflows, use a dedicated automation profile and “live in the browser” instead of decrypting cookies from disk.** The supported pattern in 2025–2026 is a separate `user-data-dir` / persistent context, one-time manual login, and then Playwright/Selenium/CDP attaching to that non-default profile. Chrome 136 explicitly tightened remote-debugging so the default profile can no longer be debugged that way. **[established]** citeturn37view1turn37view2turn36view9turn36view10
- **Do not make Brave, Arc, Opera, or Vivaldi your standard automation browser.** They remain Chromium-family enough that you inherit much of Chromium’s storage and detection behavior, but vendor documentation on cookie-storage edge cases and long-term automation ergonomics is weaker and less consistent than Firefox or Chrome/Edge. For Brave specifically, the vendor documents OSCrypt storage on all desktop OSes; for Arc/Vivaldi/Opera, I did not find equally strong current vendor docs on the exact cookie-storage and automation-facing details you need. **[established]** for Brave OSCrypt; **[no source found]** / **[speculative]** for the others’ practical long-term suitability. citeturn37view5turn31search18turn31search3
- **Your anti-bot risk is reduced most by acting like a real user on a real machine, not by piling on stealth patches.** Stable residential IP, correct locale/timezone, headful real-channel browser, warm long-lived profile, and restrained interaction patterns matter more than swapping Playwright for a stealth fork. `navigator.webdriver` is still a real signal; TLS/JA4-class network fingerprints and browser-fingerprint consistency also matter; modern defenders combine browser, transport, and behavior signals. **[established]** for `navigator.webdriver`, CDP/JA4 signal categories; **[emerging]** for exact signal weighting at Google/Meta/X. citeturn36view11turn15search2turn19search0turn19search15turn19search19
- **Expect session churn and design for relogin as maintenance, not as an exception.** Firefox makes it easy to read cookies, but it does not make upstream sessions immortal; services still expire, rotate, or step-up auth. For Google in particular, public developer/community complaints around `__Secure-1PSIDTS` expiring are real, but Google does not publish a clean contractual “these are the essential cookies” specification for every product. Treat “refresh login every few weeks” as normal. **[established]** that cookies/sessions are browser data and session cookies need proper flushing; **[single-source][emerging]** for specific Google cookie churn anecdotes. citeturn27search17turn27search13turn21search3
- **The recommended fleet standard is one main path plus one fallback.** Main path: **Firefox ESR dedicated automation profiles** for services where you need external cookie/session handoff. Fallback: **dedicated Chrome/Edge automation profiles** used only through persistent browser contexts or CDP attach, never through disk decryption, and never through the owner’s normal day-to-day browser profile. **[established]** citeturn4search1turn37view2turn37view8turn10search1turn37view1

## Browser storage map

The table below is deliberately **Windows-first**, because that is where your current blocker lives. For macOS and Linux, I note what is documented and where the evidentiary gaps are.

| Browser | Windows storage mechanism | Externally extractable by another normal process on Windows | Bypass / workaround difficulty | macOS / Linux notes |
|---|---|---:|---|---|
| **Firefox** | `cookies.sqlite` in the profile; Mozilla support points users to inspect it with SQLite tools, and geckodriver supports prepared custom profiles. There is no documented Windows ABE equivalent. **[established]** citeturn27search1turn37view3turn37view8 | **Yes, usually** for persistent cookies from the DB itself; for correctness you may need browser closure or WAL copy/flush. **[established]** / **[single-source]** on WAL/session nuances. citeturn27search1turn27search13turn27search17 | **Low** if your goal is cookie export from your own profile copy. The real operational issue is WAL/session handling, not decryption. citeturn27search13turn27search17 | Same Firefox-family model on macOS/Linux. Firefox containers and profiles are first-class Mozilla features across desktop. **[established]** citeturn37view4turn37view8 |
| **LibreWolf** | Firefox-family profile/cookie architecture is expected, but I did not find a current official LibreWolf page spelling out cookie-store internals. **[speculative][no source found]** | **Likely yes**, by Firefox-family inheritance, but verify on one machine before standardizing. **[speculative][no source found]** | **Low–medium**, depending on any LibreWolf-specific hardening you enable. **[speculative]** | Same Firefox-family caveat. **[no source found]** |
| **Chrome** | **ABE on Windows from Chrome 127 for cookies**, with enterprise policy escape hatch; Chrome 136 also stopped honoring remote debugging for the default data dir. **[established]** citeturn6search16turn10search0turn37view1 | **No**, not by trivial disk read/decrypt from another ordinary process; that is exactly what ABE is designed to stop. **[established]** citeturn6search16turn37view1 | **High** if you insist on extraction. **Low** if you instead use dedicated persistent profile + CDP / Playwright persistent context. **[established]** citeturn37view1turn37view2turn36view9turn36view10 | Google describes Windows ABE as similar to how Keychain operates on macOS. On Linux, Chromium uses libsecret/KWallet or even plain text if `--password-store=basic`. **[established]** for Linux; **[inference]** for Chrome/macOS details. citeturn28search4turn37view6 |
| **Edge** | Microsoft documents **ApplicationBoundEncryptionEnabled**, supported on **Windows ≥ 127**, enabled unless policy-disabled. **[established]** citeturn10search1turn8search9 | **No**, not by the old external-process DB-decrypt pattern when ABE is active. **[established]** by policy and design parity with Chrome. citeturn10search1turn7search10 | **High** if you want extraction; **low** if you use a dedicated Edge automation profile plus DevTools-protocol attach. **[established]** citeturn37view7turn10search1 | Microsoft’s current policy page says ABE support is **not supported on macOS**. **[established]** citeturn10search1turn8search9 |
| **Brave** | Brave officially documents Chromium **OSCrypt**: Keychain entry on macOS, DPAPI-protected `os_crypt.encrypted_key` in `Local State` on Windows, and Linux secret-store usage. **[established]** citeturn37view5 | **Mixed in practice.** Brave’s official help page still describes the older DPAPI-style Windows model; public ABE-era tools claim current Brave support. Treat current Windows extraction as **not fleet-safe** and verify per version. **[emerging][single-source]** citeturn37view5turn6search0turn7search1 | **Medium–high** because Brave is still Chromium-family, so you do not escape the general direction of travel. **[established]** for Chromium inheritance. citeturn31search1turn37view5 | Good documented cross-platform OSCrypt behavior. Still not a better long-term answer than Firefox for cookie export. **[established]** citeturn37view5turn37view6 |
| **Chromium open-source builds** | Windows upstream supports app-bound encryption primitives and policy checks in source; Linux uses libsecret/KWallet/basic. **[established]** citeturn30search3turn30search13turn37view6 | **Varies by build and flags**, but if upstream ABE is present and enabled on Windows, trivial external extraction is out. **[established]** / **[inference]** citeturn30search3turn30search13 | **Medium–high** because build/channel differences create support burden. | Linux can degrade to plain text under `--password-store=basic`, which is operationally simple but weaker. **[established]** citeturn37view6 |
| **Opera** | Chromium-derived, but I did not find a current vendor document equivalent to Brave’s “Sensitive data storage” page. **[no source found]** | **Assume not fleet-safe** for Windows cookie extraction without per-version verification. **[speculative]** | **Medium–high** because you inherit Chromium uncertainty without strong documentation. | Same caveat. Official Opera marketing pages are not enough for a storage-governance decision. **[no source found]** citeturn31search3 |
| **Vivaldi** | Chromium-derived. I found Vivaldi material for attaching DevTools via `--remote-debugging-port`, which implies normal Chromium debugging paths, but not an up-to-date vendor storage/security doc for cookies. **[partial evidence]** citeturn31search18 | **Assume not fleet-safe** for Windows cookie extraction unless you test it yourself on current builds. **[speculative]** | **Medium–high**. | Same caveat. |
| **Arc** | Chromium-derived, but I found no current vendor storage/security doc covering cookies/session encryption on Windows. Community posts indicate remote-debugging usage, not storage details. **[no source found]** citeturn31search0 | **Unknown / verify manually.** Not suitable as a standard fleet choice for your use case. **[no source found]** | **High operational uncertainty**. | Same caveat. |
| **Playwright bundled Chromium / headless builds** | Uses Chromium storage when launched with a persistent `user_data_dir`; otherwise storage can be ephemeral temp-state. Playwright explicitly warns against the default Chrome profile and recommends a separate directory. **[established]** citeturn37view2 | **Only if you own the profile and stay inside browser APIs**; not via the old “read the owner’s default Chrome DB from outside” pattern. **[established]** citeturn37view2turn37view1 | **Low** if you accept the “live in browser” model; **high** if you want disk extraction. | Detection risk is generally higher than a warmed-up real daily browser profile unless you work hard on realism. **[emerging]** citeturn19search0turn19search15 |

### What this map means in practice

The core pattern is simple: **Firefox-family remains structurally favorable to file-based automation handoff; Chromium-family is steadily becoming unfavorable on Windows.** Google says this out loud for Chrome 127, Microsoft says it out loud for Edge policy support, and Playwright now warns users not to automate the default Chrome profile at all. **[established]** citeturn6search16turn10search1turn37view1turn37view2

The honest uncertainty is around the long tail of Chromium derivatives. Brave documents OSCrypt very clearly, but not every derivative publishes equally good storage docs. So for **Brave/Opera/Vivaldi/Arc**, the correct posture is not “probably fine” but **“not a standard; verify on one sacrificial machine or avoid.”** **[established]** / **[no source found]** citeturn37view5turn31search18turn31search3

## Firefox as the automation browser

### Why Firefox fits your use case

Firefox gives you three layers of separability that are all useful, but they solve different problems.

A **profile** is the heavy isolation unit: bookmarks, passwords, extensions, browsing data, and settings. Mozilla explicitly recommends multiple profiles to separate work and personal use or test setups, and geckodriver supports both throwaway and custom prepared profiles. **[established]** citeturn37view8turn37view3

A **container** is the lighter isolation unit inside a profile: color-coded tab spaces with isolated site identity, allowing different logins to the same site or to different sites within one top-level profile. Mozilla documents that each container is isolated from the others and that the Multi-Account Containers add-on adds site assignments, sync, and proxy/VPN integration. **[established]** citeturn37view4turn36view5

For your scenario, the clean rule is this:

- **Use separate profiles for different risk domains or major services** such as `google-main`, `meta-main`, `x-main`, `telegram-main`.
- **Use containers only when you truly need multiple accounts of the same service inside one service-domain profile** and want a human operator to be able to see or repair that state easily.

That is simpler to explain to a non-technical owner than a maze of stealth patches and CDP ports. **[established]** for the capabilities; **[inference]** for the recommended operating model. citeturn37view8turn37view4turn36view5

### What is stable and what is not

The good news is that the Firefox operational risk is mostly **not decryption**. The main gotchas are **profile locking**, **WAL/SHM journal files**, and the fact that Mozilla support contributors note that `cookies.sqlite` only covers persistent cookies while session visibility can depend on proper flushing. **[single-source]** citeturn27search13turn27search17

So the practical lesson is:

- if you need a **clean cookie export**, prefer an orderly browser close and then read a copied profile DB;
- if the browser remains open, copy `cookies.sqlite` **together with** `cookies.sqlite-wal` and `cookies.sqlite-shm` when present;
- treat “extract from a hot live DB and expect perfect completeness” as a trap.

Operationally, that matters far more than schema churn. Mozilla support clearly treats `cookies.sqlite` as the place users inspect with SQLite tools, which is exactly why Firefox is attractive for your pattern. **[established]** / **[single-source]** citeturn27search1turn27search13turn27search17

### Geckodriver, Marionette, profiles, and headless

Mozilla’s geckodriver documentation says it normally creates a temporary throwaway profile, but it also explicitly supports custom prepared profiles. It can use them either via `--profile /some/location` or a Base64-zipped profile capability. Mozilla also notes a known bug around `--profile` and randomized Marionette ports, with the documented workaround being an explicit `--marionette-port`. **[established]** citeturn37view3

That is strong evidence for a maintainable Firefox automation stack:

- **Prepared profile folders** for long-lived state.
- **geckodriver / Selenium** when you want classic WebDriver / Marionette control.
- **Playwright Firefox persistent context** when you want one-time login and then long-lived browser state kept on disk in a dedicated directory. Playwright documents persistent contexts generically for session data such as cookies and local storage. **[established]** citeturn37view2turn37view3

Headless Firefox is supported through normal browser automation arguments and browser-specific Selenium features, but for anti-abuse-sensitive consumer services I would still favor **headful Firefox on the always-on Windows box** unless you prove the site tolerates headless. Selenium’s Firefox browser-specific docs list `-headless` and `-profile` among common arguments. **[established]** citeturn26search11turn37view3

### How I would isolate multiple accounts of one service

For **Google NotebookLM + Gmail + other Google web apps**, I would not put multiple Google identities in one Firefox profile unless a human operator absolutely needs that convenience. Google properties are heavily cross-wired through auth and anti-abuse systems; clean repair is easier when each Google identity is its own profile. **[inference]**

For **Meta/Facebook** and **X**, separate profiles are also cleaner than containers if those accounts are business-critical, because an operator can launch a named desktop shortcut, see one browser identity, and repair one problem at a time. Containers are best as a second-level tool, not the primary maintenance boundary. Mozilla’s own docs describe profiles as the right boundary for separate work/personal/test setups and containers as the tab-level identity splitter. **[established]** citeturn37view8turn37view4

## Chromium options and why most bypasses are bad architecture

### What Chrome ABE changed

Google’s July 2024 security announcement was explicit: **Chrome 127 on Windows introduced App-Bound Encryption for cookies**, tying decryption to app identity instead of letting any same-user process unwrap browser secrets via the old user-bound pattern. Google also shipped an enterprise policy named `ApplicationBoundEncryptionEnabled` for compatibility scenarios and emits **Event ID 257** from the `Chrome` source when verification fails. **[established]** citeturn6search16turn10search0

Microsoft documents the same idea for Edge: **ApplicationBoundEncryptionEnabled** supported on **Windows ≥ 127**, enabled by default unless policy-disabled, with the explicit warning that disabling it reduces security. Microsoft’s policy page also says ABE is **not supported on macOS** for Edge. **[established]** citeturn10search1turn8search9

That means your observed failure mode—`rookiepy` / `browser_cookie3` suddenly returning “could not decrypt” on modern Chrome/Edge Windows profiles—is not incidental breakage. It is the expected result of the new browser security model. **[established]** citeturn6search16turn10search1

### The known “bypass” families in 2026

The public bypass families break down into four buckets.

The first is **owner-process / elevation-service abuse**: projects such as `Chrome-App-Bound-Encryption-Decryption`, `ZeroCrumb`, and newer ABE-decryption repos publicly claim decryption on Chrome, Edge, and Brave via browser-identity abuse, COM/elevation service interfaces, process hollowing, or DLL injection. Some public research specifically discusses **Chrome 144 / IElevator2** and Edge-specific variants. **[emerging][single-source]** citeturn6search11turn6search14turn7search1turn6search17turn6search0

The second is **remote debugging / CDP extraction**. Google’s March 2025 Chrome-for-Developers post explicitly says attackers increasingly used remote debugging to extract cookies after ABE rollout. Chrome then changed `--remote-debugging-port` and `--remote-debugging-pipe` so they are **ignored for the default Chrome data directory** unless paired with a **non-standard `--user-data-dir`**. **[established]** citeturn37view1turn36view8

The third is **“disable ABE” through managed policy**. This is supported, but only as an enterprise policy escape hatch; both Google and Microsoft frame it as a compatibility knob with security cost, not as a normal automation technique. Chromium source also notes that disabling the policy does **not** disable decryption of already-existing protected data. **[established]** citeturn10search0turn10search1turn30search13

The fourth is the bucket people usually hand-wave with flags like `--disable-features=...`. I did **not** find a current official supported `--disable-features=` recipe for turning Chrome ABE off. The supported mechanism I did find is the managed policy. **[no source found]**

### What is actually worth doing

For your environment, only two Chromium patterns survive a repairability test.

The first is **persistent browser profile automation**: create a **separate automation profile**, login once manually, then keep using that profile via Playwright/Selenium. Playwright documents this directly and warns against using the owner’s real default Chrome profile. **[established]** citeturn37view2

The second is **CDP attach to an already-running dedicated automation browser**: this is still viable for your own non-default profile. The DevTools protocol exposes cookie-reading methods; `Network.getAllCookies` is deprecated in favor of `Storage.getCookies`. **[established]** citeturn36view9turn36view10

By contrast, **COM abuse, process hollowing, and stealth decryption projects are not a standard architecture**. Even if they work on your own hardware today, they create a maintenance stack that looks like malware-analysis tooling. That is bad for reliability, bad for owner comprehension, and likely bad for EDR/AV. **[inference]** supported by the nature of the published techniques. citeturn6search11turn6search17turn7search1

## Detection, session durability, and safety boundaries

### What tends to trigger anti-bot systems

Modern bot defense is not one signal. It is a **stack**.

At the browser/API layer, `navigator.webdriver` is still a standard disclosure signal. MDN documents that it becomes `true` in Chrome under `--enable-automation` or `--headless`, and in Firefox when Marionette is enabled. **[established]** citeturn36view11

At the transport/network layer, vendors such as Cloudflare expose **JA4**-style fingerprint intelligence as a real signal in bot scoring. **[established]** citeturn15search2

At the JavaScript/browser-fingerprint layer, specialist detection research in 2025 highlights HTTP-header anomalies, JavaScript fingerprint inconsistencies, Canvas/WebGL/device-surface mismatches, and CDP/automation artifacts as meaningful signals. Castle’s 2025 assessment also argues that the simple era of “one stealth plugin and done” is over; modern anti-detect tools are trying to close inconsistencies across multiple layers. **[emerging]** citeturn19search0turn19search11turn19search15turn19search19

At the behavior and account-reputation layer, platforms look at posting frequency, spam-like behavior, and authenticity / fake-account indicators. Meta’s transparency materials emphasize fake-account and spam enforcement using automated systems; X’s automation rules make the account owner responsible for what automation does and say account action can follow. **[established]** citeturn25search6turn25search14turn25search16turn25search1turn25search4

### What helps more than stealth patches

For your setup, the biggest wins are boring:

- **Residential IP with geographic consistency**.
- **Timezone, locale, browser language, and OS region aligned with that IP**.
- **Headful, real browser channel** instead of a just-spawned lab browser.
- **Warm, long-lived profile** with normal browsing residue for that account.
- **Moderate cadence** rather than bursts of repeated actions.

That is why your “always-on Windows 11 machine, residential IP, no VPN” is already better than many automation stacks. The same detection literature that discusses stealth frameworks also notes that bots now use residential proxies and anti-detect frameworks precisely because defenders score more than WebDriver alone. A stable real machine remains the saner side of that tradeoff. **[emerging]** citeturn19search15turn19search19turn15search2

This is also why a **real logged-in Firefox or Chrome stable channel** is usually safer than a fresh bundled headless Chromium. That is an inference, but it is strongly supported by the fact that defenders combine JS surface, CDP artifacts, and transport fingerprints, while stock headful-browser traffic better matches normal user baselines. **[emerging]** citeturn19search0turn19search11turn19search17

### How to think about stealth tools in 2026

`undetected-chromedriver` still exists, but the project’s own ecosystem now points to **nodriver** as its successor; public discussions in 2026 ask for maintained forks, and the official `nodriver` repo calls itself the successor. That is a sign of churn, not stability. **[established]** citeturn33search0turn33search2turn33search3

`playwright-stealth` still exists, but the current PyPI page explicitly says it should be treated as a proof-of-concept that should not be expected to beat anything but the simplest detections. Issue trackers also show detection complaints on real test sites. **[established]** citeturn34search4turn34search6turn34search12

`patchright` is actively positioned as a patched Playwright for Chromium only, and `camoufox` positions itself as a Firefox-based anti-detect browser. Both are interesting research tools; neither is a maintenance-free fleet standard. **[established]** that they position themselves that way; **[speculative]** on their long-term sustainability for your case. citeturn32search1turn32search0turn32search4

My recommendation is blunt: **treat these as exceptional fallback tools for specific hostile sites, not as the default browser layer for personal-account productivity automation.**

### Session durability and why relogin still happens

Firefox does not solve session expiry; it only solves your local access to stored state.

Mozilla support contributors note that `cookies.sqlite` stores persistent cookies and that session handling can depend on flush timing. That alone means a raw DB export is not the whole state model for every site. **[single-source]** citeturn27search17turn27search13

For Google specifically, public community threads around Gemini/Google-web automation report frequent `__Secure-1PSIDTS` expiry or churn. I do **not** treat those threads as a contractual Google reference for which cookies are necessary, but they are useful evidence that your “every couple of weeks” experience is not unusual. **[single-source][emerging]** citeturn21search3

The practical rule is:

- design a **keepalive routine** that opens the browser profile and performs one or two ordinary, human-legible actions for each service on a schedule;
- expect occasional **step-up auth** anyway;
- separate your **human primary profile** from your **automation profile** so reauth on the latter does not contaminate the former.

That is more robust than chasing “master token” fantasies from reverse-engineered APIs. **[inference]**

### Safety and ToS boundaries

Google’s general Terms forbid abusing, interfering with, or bypassing protective measures, and specifically mention deceptive access patterns and bypassing protective measures. That does **not** mean “all personal-account automation is automatically banned,” but it does mean your safest path is a narrow one: automate your own accounts, act as yourself, do not fake identities, do not scrape third-party private content, and do not try to defeat security controls. **[established]** citeturn37view0

X’s automation rules say the account owner is responsible for activity taken with the account and that account action can follow if automation violates rules. Meta’s enforcement materials focus on fake accounts, spam, and inauthentic behavior. That again points toward the same operational boundary: **own account, owner-benefiting productivity, quiet behavior, no impersonation, no mass actions.** **[established]** citeturn25search1turn25search4turn25search6turn25search16

For secret handling, extracted cookies are **credentials**. X’s authenticity/help materials explicitly list cookies, tokens, and keys among login data that can compromise an account if shared. Store them like passwords: OS-protected secret store if possible, otherwise encrypted at rest with an access-controlled service or vault. **[established]** citeturn25search2

## Recommended architecture for your fleet

### The main path

**Standardize on Firefox ESR as the dedicated automation browser on all human-operated desktop nodes where you need external cookie/session handoff.** **[established]**

Concretely, for the always-on Windows 11 hub:

- install **Firefox ESR 64-bit**;
- create **one profile per major identity / service cluster** rather than one mega-profile;
- use **containers only inside a profile** when you truly need more than one login for the same service;
- keep this browser **separate from the owner’s normal browsing**;
- for code that needs cookies outside the browser, read from the Firefox profile copy or use the library path you already proved works in your environment;
- for web-driving tasks, use **Playwright Firefox persistent context** or **geckodriver with prepared profiles**. citeturn4search1turn37view3turn37view4turn37view8

A simple folder convention keeps the hammer-and-screwdriver repair model intact:

```text
D:\AutomationBrowsers\
  Firefox\
    google-main\
    google-alt\
    meta-main\
    x-main\
    telegram-main\
  Chrome\
    notebooklm-fallback\
    chrome-only-fallback\
```

Then give the owner **desktop shortcuts with obvious names** like:

```text
Firefox - Google Main
Firefox - Meta Main
Firefox - X Main
Firefox - Telegram Main
Chrome - NotebookLM Fallback
```

The operator’s repair playbook becomes: “Open the named shortcut, see if login is alive, relogin if needed, close it cleanly.” That is the right level of complexity. **[inference]**

### The fallback

**Use dedicated Chrome or Edge automation profiles only for Chromium-only compatibility, and never for disk decryption.** **[established]**

That means:

- the profile is **not** the owner’s normal default browser profile;
- it lives in a **separate `user-data-dir`**;
- the automation connects via **persistent context** or **CDP attach**;
- if cookies are needed, obtain them **through the browser protocol** rather than by file-level decryption from an external normal process. citeturn37view1turn37view2turn36view9turn36view10

For Windows 11, the manual-launch pattern is:

```bat
chrome.exe --user-data-dir=D:\AutomationBrowsers\Chrome\notebooklm-fallback --remote-debugging-port=9222
```

That respects Chrome’s 2025 hardening because the profile is **non-default**. **Do not** point this at the owner’s main Chrome “User Data” folder. **[established]** citeturn37view1turn37view2

Then attach from Playwright:

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.connect_over_cdp("http://127.0.0.1:9222")
    context = browser.contexts[0]
    page = context.new_page()
    page.goto("https://notebooklm.google.com/")
```

And if you need cookie state from that browser context, prefer the browser protocol’s cookie APIs conceptually equivalent to `Storage.getCookies`, because `Network.getAllCookies` is deprecated. **[established]** citeturn36view9turn36view10

### Concrete Windows and Python setup

For **Firefox / Playwright persistent context**:

```python
from pathlib import Path
from playwright.sync_api import sync_playwright

PROFILE = Path(r"D:\AutomationBrowsers\Firefox\google-main")

with sync_playwright() as p:
    context = p.firefox.launch_persistent_context(
        user_data_dir=str(PROFILE),
        headless=False,
    )
    page = context.new_page()
    page.goto("https://accounts.google.com/")
    # First run: log in manually.
    input("After login is complete, press Enter to continue...")
    context.close()
```

For **Firefox cookie export after a clean close**:

```python
from pathlib import Path
import shutil
import sqlite3
import json

profile = Path(r"D:\AutomationBrowsers\Firefox\google-main")
src = profile / "cookies.sqlite"
tmp = profile / "cookies.copy.sqlite"

shutil.copy2(src, tmp)

con = sqlite3.connect(f"file:{tmp.as_posix()}?mode=ro", uri=True)
rows = con.execute("""
    SELECT host, path, name, value, expiry, isSecure, isHttpOnly, sameSite
    FROM moz_cookies
""").fetchall()

cookies = [
    {
        "domain": host,
        "path": path,
        "name": name,
        "value": value,
        "expiry": expiry,
        "secure": bool(is_secure),
        "httpOnly": bool(is_http_only),
        "sameSite": same_site,
    }
    for host, path, name, value, expiry, is_secure, is_http_only, same_site in rows
]

print(json.dumps(cookies[:5], indent=2))
con.close()
tmp.unlink(missing_ok=True)
```

If you must extract while Firefox is still open, copy `cookies.sqlite`, `cookies.sqlite-wal`, and `cookies.sqlite-shm` together when present; otherwise you risk stale reads. Mozilla support forum guidance on SQLite WAL files is the relevant operational clue here. **[single-source]** citeturn27search13turn27search17

If you prefer `rookiepy`, the architecture still remains the same: **Firefox dedicated profile as source of truth**, not “whatever browser the owner happened to use today.”

### Should Firefox be the standard across hub, laptops, Macs, and VPS

My answer is **yes for desktops/laptops where cookie export is a requirement, no for VPS as the universal login anchor.** **[established]** / **[inference]**

Use Firefox as the standard on:

- the always-on Windows 11 hub,
- human-operated Windows laptops,
- Macs used for the same owner-operated profile workflows.

Do **not** make a Linux VPS the canonical home of sensitive consumer-service sessions unless you need it for something Chrome/Firefox can only do there. VPS environments tend to be worse at “ordinary consumer user” signals; your residential always-on hub is the better anchor. **[emerging]** citeturn19search15turn15search2

The fleet policy I would actually write is:

> **Default:** Firefox ESR dedicated automation profiles for all cookie-export / reverse-engineered API handoff use cases.  
> **Fallback:** dedicated Chrome or Edge automation profile with persistent context / CDP for Chrome-only compatibility.  
> **Not standard:** Brave, Arc, Opera, Vivaldi, anti-detect forks.  
> **Exception-only:** patchright / camoufox / nodriver / undetected-chromedriver for a specifically blocked site, after proving business need.

### The main traps and how to avoid them

The biggest trap is trying to automate the **owner’s real default Chrome profile**. Chrome and Playwright both moved against that. Use a separate automation profile. **[established]** citeturn37view1turn37view2

The next trap is treating Firefox DB access as magically complete while the browser is hot. It is simpler than Chrome decryption, but WAL/session behavior still matters. Close cleanly or copy the journal files too. **[single-source]** citeturn27search13turn27search17

The third trap is overusing containers where profiles are the correct boundary. Containers are brilliant for tab-level identity split, but profiles are easier to repair, back up, and explain. Mozilla’s own docs reflect that separation of roles. **[established]** citeturn37view4turn37view8

The fourth trap is accepting stealth-tool churn as normal infrastructure. It is not. The current ecosystem itself tells the story: `undetected-chromedriver` → `nodriver`; `playwright-stealth` says proof of concept; `patchright` and `camoufox` market themselves as anti-detect forks. That is useful as a lab, not as a park standard. **[established]** citeturn33search3turn34search4turn32search1turn32search0

The fifth trap is treating cookies as “just session state.” They are credentials. Store them accordingly and isolate the automation browser from the owner’s personal browser. X’s help center explicitly lists cookies, tokens, and keys as compromise-sensitive login data. **[established]** citeturn25search2

## Source list

```text
Primary and high-value sources used in this report
Accessed: 2026-07-16 unless noted otherwise

Google Security Blog — Improving the security of Chrome cookies on Windows — 2024-07-30
https://security.googleblog.com/2024/07/improving-security-of-chrome-cookies-on.html

Chrome for Developers — Changes to remote debugging switches to improve security — 2025-03-17
https://developer.chrome.com/blog/remote-debugging-port

Chrome DevTools Protocol — Network domain
https://chromedevtools.github.io/devtools-protocol/tot/Network/

Chrome DevTools Protocol — Storage domain
https://chromedevtools.github.io/devtools-protocol/tot/Storage/

Playwright Python docs — BrowserType / launchPersistentContext
https://playwright.dev/python/docs/api/class-browsertype

Mozilla Support — Manage Firefox profiles — 2026-06-12
https://support.mozilla.org/en-US/kb/profile-management

Mozilla Support — Multi-Account Containers
https://support.mozilla.org/en-US/kb/containers

Mozilla Support — How to use Firefox containers — updated 2026
https://support.mozilla.org/en-US/kb/how-use-firefox-containers

Firefox Source Docs — geckodriver Profiles
https://firefox-source-docs.mozilla.org/testing/geckodriver/Profiles.html

MDN — Navigator: webdriver property — 2024-11-05
https://developer.mozilla.org/en-US/docs/Web/API/Navigator/webdriver

Chromium Docs — Linux Password Storage
https://chromium.googlesource.com/chromium/src/+/master/docs/linux/password_storage.md

Chromium source / docs — app_bound_encryption_win and OSCrypt references
https://chromium.googlesource.com/chromium/src/+/refs/tags/127.0.6533.81/chrome/browser/os_crypt
https://github.com/chromium/chromium/blob/main/chrome/browser/os_crypt/app_bound_encryption_win.cc

Microsoft Learn — Edge policy ApplicationBoundEncryptionEnabled — 2026-05-21 / 2026-05-22
https://learn.microsoft.com/en-us/deployedge/microsoft-edge-policies/applicationboundencryptionenabled

Microsoft Learn — Microsoft Edge DevTools Protocol — 2025-06-20
https://learn.microsoft.com/en-us/microsoft-edge/devtools/protocol/

Brave Help Center — Sensitive data storage — 2024-08-30
https://support.brave.app/hc/en-us/articles/29808985123085-Sensitive-data-storage

Google Terms of Service — effective 2024-05-22; preview update noted for 2026-07-30
https://policies.google.com/terms

X Help Center — X's automation development rules — crawled 2026
https://help.x.com/en/rules-and-policies/x-automation

X Help Center — About automated account labels
https://help.x.com/en/using-x/automated-account-labels

X Help Center — Authenticity
https://help.x.com/en/rules-and-policies/authenticity

Meta Transparency Center — Fake Accounts enforcement report
https://transparency.meta.com/reports/community-standards-enforcement/fake-accounts/

Meta Transparency Center — Account Integrity
https://transparency.meta.com/policies/community-standards/account-integrity/

Meta Transparency Center — Spam
https://transparency.meta.com/policies/community-standards/spam/

Mozilla Support Forum — cookies.sqlite inspection via SQLite — 2020-11-18
https://support.mozilla.org/en-US/questions/1314237

Mozilla Support Forum — cookies.sqlite WAL/SHM explanation — 2018-08-10
https://support.mozilla.org/ml/questions/1229165

Mozilla Support Forum — cookies.sqlite only stores persistent cookies — 2022-10-30
https://support.mozilla.org/da/questions/1395023

Specialist / community research used with reduced trust and labeled accordingly

Castle Research Blog — How to detect Headless Chrome bots instrumented with Playwright — 2025-03-19
https://blog.castle.io/how-to-detect-headless-chrome-bots-instrumented-with-playwright/

Castle Research Blog — Bot detection 101: How to detect bots in 2025 — 2025-03-25
https://blog.castle.io/bot-detection-101-how-to-detect-bots-in-2025-2/

Castle Research Blog — Why a classic CDP bot detection signal suddenly stopped working — 2025-08-28
https://blog.castle.io/why-a-classic-cdp-bot-detection-signal-suddenly-stopped-working-and-nobody-noticed/

Castle Research Blog — Why traditional bot detection techniques are not enough — 2025-04-09
https://blog.castle.io/why-traditional-bot-detection-techniques-are-not-enough-and-what-you-can-do-about-it/

Cloudflare docs — Signals Intelligence / JA4 fingerprints — 2026-05-06
https://developers.cloudflare.com/bots/additional-configurations/ja3-ja4-fingerprint/signals-intelligence/

xaitax — Chrome App-Bound Encryption Decryption research docs — accessed 2026-07-16
https://github.com/xaitax/Chrome-App-Bound-Encryption-Decryption
https://github.com/xaitax/Chrome-App-Bound-Encryption-Decryption/blob/main/docs/RESEARCH.md
https://github.com/xaitax/Chrome-App-Bound-Encryption-Decryption/blob/main/docs/The_Elevator_Gets_an_Upgrade_Chrome_144_IElevator2_and_the_Mojo_Horizon.md
https://github.com/xaitax/Chrome-App-Bound-Encryption-Decryption/blob/main/docs/The_Curious_Case_of_the_Cantankerous_COM_Decrypting_Microsoft_Edge_ABE.md

Majanito — ABE-Decryption repo
https://github.com/Majanito/ABE-Decryption

ZeroCrumb repo
https://github.com/dk0m/ZeroCrumb

undetected-chromedriver repo and related discussions
https://github.com/ultrafunkamsterdam/undetected-chromedriver
https://github.com/ultrafunkamsterdam/nodriver

Patchright repo
https://github.com/Kaliiiiiiiiii-Vinyzu/patchright

Camoufox repo and project site
https://github.com/daijro/camoufox
https://camoufox.com/

playwright-stealth project and PyPI page
https://github.com/Granitosaurus/playwright-stealth
https://pypi.org/project/playwright-stealth/

Google-cookie community thread used only as anecdotal evidence of churn
https://github.com/HanaokaYuzu/Gemini-API/issues/6
```