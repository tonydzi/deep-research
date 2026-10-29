---
dr_id: DR26-07-03-HUB-03
title: "Keeping an unattended Windows 11 AI workstation authenticated for 45 days"
date: 2026-07-03
lang: en
source: Palo Alto AI Research Lab — deep research programme
---

# Insight (DR DR26-07-03-HUB-03): Keeping an unattended Windows 11 AI workstation authenticated for 45 days

> The report answers how to keep Google browser sessions, Gmail OAuth tokens, and the Windows desktop session alive (plus rescue paths) on an unattended Windows 11 PC for 45 days without anyone present.

## Ключевые выводы
- Google does not publish a fixed TTL for consumer Chrome web sessions; Workspace accounts default to a 14-day session (admin-configurable, can be set to never expire) — the real lever is avoiding re-auth triggers, not timing a refresh
- Cookie persistence is the actual session: Google's 'trusted computer'/'stay signed in' mechanism depends entirely on cookies surviving — antivirus/cleaners deleting cookies breaks it
- Google's risk engine re-challenges sign-ins from unfamiliar device/location/network — keep the machine on the same device, browser profile, and stable network path throughout
- Google imposes a 7-day cooldown on trust for new factors (device, phone, security key, passkey) — all identity setup (recovery number, passkey, prompt) must be finished at least 7 days before the unattended period starts
- Password changes sign the user out almost everywhere and revoke Gmail-scoped OAuth refresh tokens — no password changes during the unattended window
- Gmail refresh tokens are revoked by: app revocation, 6 months unused, password change (with Gmail scope), exceeding 100 refresh tokens per account per client ID (oldest gets silently invalidated), or expired time-based grants; OAuth apps in 'Testing' status only get 7-day tokens vs Production's long-lived tokens
- Chrome's new App-Bound Encryption and Google's Device Bound Session Credentials increasingly tie cookies/sessions to the specific device+app identity, making 'copy cookies elsewhere' an increasingly unreliable backup strategy — must preserve the exact same machine/user/profile
- Windows update pause caps at 35 days but can be re-extended; a more robust control is 'Configure Automatic Updates = 2' (notify only, never auto-download/install) plus pinning the feature release via ProductVersion/TargetReleaseVersion, since metered connection only blocks 'some' updates
- Sysinternals Autologon (LSA-secret based, safer than plaintext registry AutoAdminLogon) plus disabling sleep and 'sign-in on wake' ensures a surprise reboot still lands back in the same interactive desktop where Chrome can decrypt cookies and GUI automation can run (Task Scheduler's InteractiveToken requires an existing interactive session)
- WhatsApp linked devices log out if the primary phone is unused >14 days; Telegram (Telethon) and WhatsApp (Baileys) both rely on durably persisted session/credential files that must never be regenerated or leaked
- Best rescue layering: Chrome Remote Desktop or RustDesk-over-Tailscale for console access while Windows is alive, plus a hardware KVM with ATX power control (e.g. PiKVM) for BIOS/BitLocker/boot-failure scenarios where software remote access can't help; BitLocker recovery key must be accessible from the owner's phone

## Рекомендации / решения
- Put each critical Google account in its own Chrome profile; keep cookies enabled, disable cookie-clearing tools, check 'don't ask again on this computer'
- Complete all Google trust-building (passkey, recovery phone, Google Prompt enrollment, trusted device) at least 7 days before the unattended period begins, then leave it untouched
- Do not change primary account passwords or generate new OAuth consent grants during the unattended window; reuse one durable production refresh token per mailbox
- Keep the OAuth client in Production status (never revert to Testing) and use it regularly so tokens/client aren't flagged inactive
- Pin the Windows 11 feature release (ProductVersion + TargetReleaseVersion) and set Configure Automatic Updates=2 (notify only) rather than relying solely on the 35-day update pause
- Enable Sysinternals Autologon, disable sleep, set sign-in-on-wake to Never, and launch all browser/GUI automation only after interactive logon (not via non-interactive/S4U task contexts)
- Verify Telegram .session file and WhatsApp Baileys auth state survive a reboot before departure
- Set up a two-tier remote rescue plan: Chrome Remote Desktop or RustDesk/Tailscale for live console access, plus a PiKVM (or similar) with ATX power control for boot-level failures
- Store the BitLocker recovery key somewhere accessible from the owner's phone
- Avoid VPN/IP rotation or geographic changes during the unattended period — keep network path stable to avoid tripping Google's risk-based re-auth

## Сущности
- **Люди:** —
- **Компании:** Google, Microsoft, Anthropic
- **Продукты/инструменты:** Windows 11 Pro, Google Chrome, Google Workspace, Gmail API, OAuth 2.0, Sysinternals Autologon, Google Prompt, Google passkeys, Device Bound Session Credentials, App-Bound Encryption, DPAPI, Cross-Account Protection (RISC), Telethon, Telegram, WhatsApp, Baileys, Chrome Remote Desktop, Tailscale, RustDesk, PiKVM, BitLocker, Task Scheduler, Policy CSP (ProductVersion/TargetReleaseVersion)

## Открытые вопросы
- No public Google document confirms a fixed TTL for consumer desktop-Chrome web sessions
- Unclear whether a routine Chrome version update alone forces Google re-authentication
- Google does not publicly enumerate 'automation-like activity' as a specific re-auth trigger, so there's no guaranteed-safe browser-automation pattern
- Whether periodic automated activity in an already-authenticated profile helps or harms session longevity is not directly documented (only inferred from 'familiarity helps' guidance)

## Источник
- DR-ID `DR26-07-03-HUB-03` · реестр _DR-Registry
- оригинал: «внутренний путь лаборатории»

## Связано
- desktop-max-laptop-min
- credential-store
- machine-bus-telegram-rail
- chrome-autonomy-self-drive
- config-safety-backup-and-migration-check
- social-auth-autonomous
- telegram-otp-self-fetch
- system-architect
- insight-DR-DR26-07-04-HUB-03-keeping-an-unattended-windows-11-ai-workstation-go — почти дубль-DR того же вопроса днём ранее
