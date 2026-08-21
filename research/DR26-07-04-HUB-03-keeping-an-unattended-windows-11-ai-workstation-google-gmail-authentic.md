---
dr_id: DR26-07-04-HUB-03
title: "Keeping an unattended Windows 11 AI workstation Google/Gmail-authenticated for 45 days"
date: 2026-07-04
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-04-HUB-03): Keeping an unattended Windows 11 AI workstation Google/Gmail-authenticated for 45 days

> Research on how to keep a Windows 11 hub's Google web sessions, Gmail OAuth tokens, and Windows session alive and recoverable during 45 days with no owner physically present.

## Ключевые выводы
- Google publishes no fixed TTL for consumer desktop Chrome web sessions; only Workspace accounts have a documented default of 14 days (admin-configurable, can be set to never expire) — the real lever is avoiding re-auth triggers, not timing a refresh.
- Session survival depends on cookie persistence and device familiarity: keep the same Chrome profile, same device, same browser, same network path/location; unfamiliar location/device or sensitive actions trigger 'Verify it's you' challenges.
- Google imposes a 7-day cool-down on trust for new factors (device, phone number, security key, passkey) — all identity setup (recovery phone, passkey, Google Prompt) must be finished at least 7 days before the unattended period starts.
- Password changes sign the account out everywhere (except verifying devices) and revoke Gmail-scoped OAuth refresh tokens — no password changes during the unattended window.
- Gmail API refresh tokens are revoked by: app revocation, 6 months unused, password change (Gmail scopes), exceeding 100 refresh tokens per account per OAuth client ID (oldest token silently invalidated), or expired time-based grants; OAuth apps in 'Testing' status get only 7-day tokens, so the client must stay in Production.
- No public evidence that periodic 'keepalive' automated browsing extends session life; conversely, automation that mimics fresh sign-ins or touches sensitive account pages can itself trigger risk challenges — ordinary use in the same authenticated profile is safe, synthetic re-login patterns are not.
- Chrome's App-Bound Encryption and Google's emerging Device Bound Session Credentials tie cookies/sessions to the specific machine+app identity, so copying cookie/profile data to another machine as a backup plan is increasingly unreliable — must preserve the same machine + same Windows user + same Chrome profile.
- Windows Update pause tops out at 35 days (renewable) but is not fully autonomous; safer is policy-based prevention: pin the feature release (ProductVersion+TargetReleaseVersion, deferrable up to 365 days) and set 'Configure Automatic Updates=2' (notify only, no auto-download/install), with metered connection as a secondary layer.
- Sysinternals Autologon (LSA-secret-based, safer than raw registry AutoAdminLogon) plus disabling sleep and 'sign-in on wake' ensures a surprise reboot still lands back in the same interactive user desktop, which Chrome needs to decrypt cookies (DPAPI/App-Bound Encryption) and which GUI automation needs (Task Scheduler's InteractiveToken only runs in a logged-on session).
- For rescue from the owner's phone: Telegram sessions persist via .session files (must be backed up, never regenerated), WhatsApp linked devices log out if the primary phone is unused >14 days, and a layered remote-access plan (Chrome Remote Desktop or RustDesk-over-Tailscale for a live console, PiKVM with ATX power control for BIOS/BitLocker/boot failures) is needed since BitLocker recovery can be triggered by security/hardware events.

## Рекомендации / решения
- Put each critical Google account in its own dedicated Chrome profile; disable any cookie-clearing tools/antivirus behavior that could wipe Google cookies; keep 'Don't ask again on this computer' checked.
- Finish all Google trust-building (recovery phone, passkey, Google Prompt enrollment, trusted-device confirmation) at least 7 days before the unattended period begins, then leave it untouched.
- Do not change the Google account password and do not mint new OAuth consent grants/refresh tokens during the unattended window; keep the OAuth client in Production status and use the existing refresh token regularly.
- Pin the current Windows 11 feature release via ProductVersion/TargetReleaseVersion, set Configure Automatic Updates=2 (notify only), and optionally mark the network connection as metered — do not rely solely on the 35-day update pause.
- Enable Sysinternals Autologon, disable sleep, set 'require sign-in on wake' to Never, and launch all browser/GUI automation only after interactive logon (not via non-interactive/S4U scheduled tasks).
- Build a rescue ladder from the owner's phone: primary Chrome Remote Desktop or RustDesk-over-Tailscale for live console access, secondary PiKVM with ATX power control for BIOS/BitLocker/boot-failure scenarios; store the BitLocker recovery key somewhere accessible from the phone.
- Verify before departure that Telegram .session files and WhatsApp/Baileys auth state survive reboot, and that the primary phone stays in active WhatsApp use (no >14-day idle) during the trip.

## Сущности
- **Люди:** —
- **Компании:** Google, Microsoft, Anthropic
- **Продукты/инструменты:** Chrome, Gmail API/OAuth, Google Workspace, Windows 11 Pro, Sysinternals Autologon, BitLocker, Telegram/Telethon, WhatsApp/Baileys, Chrome Remote Desktop, Tailscale, RustDesk, PiKVM, Task Scheduler, DPAPI/App-Bound Encryption, Device Bound Session Credentials, Cross-Account Protection/RISC

## Открытые вопросы
- No public documentation of a fixed consumer desktop-Chrome session TTL for Google Account web sessions.
- No public confirmation that a routine Chrome version update alone forces Google re-authentication.
- Google does not publicly enumerate 'automation-like activity' as a specific re-auth risk trigger, so there's no guaranteed-safe pattern for browser automation frequency/behavior.
- Whether periodic synthetic activity provides any durability benefit remains unproven either way (only inferred from risk-familiarity documentation).

## Источник
- DR-ID `DR26-07-04-HUB-03` · реестр [[_DR-Registry]]
- оригинал: `E:\Obsidian\_originals\2026-07-03-DR26-07-04-HUB-03-away-mode-45days-unattended-auth.md`

## Связано
- [[away-mode-hub-hardening]]
- [[google-oauth-token-lifecycle]]
- [[windows-update-policy-control]]
- [[unattended-workstation-rescue-plan]]
- [[chrome-session-persistence]]
- [[machine-bus-telegram-rail]]
- [[credential-store]]
- [[insight-DR-DR26-07-03-HUB-03-keeping-an-unattended-windows-11-ai-workstation-au]] — почти дубль-DR днём позже по той же теме away-mode
