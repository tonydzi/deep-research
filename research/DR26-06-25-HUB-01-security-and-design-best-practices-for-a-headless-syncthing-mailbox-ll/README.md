# How DR26-06-25-HUB-01 was run

- topic: Security and Design Best Practices for a Headless Syncthing-Mailbox LL (ЗАКРЫТО 01.08 суждением: главный вывод отчёта — LLM-классификатор GREEN/RED НЕ может быть границей авторизации, дверь обязана быть детерминированной и в коде приложения — принят и реализован. След: decision-2026-07-14-injection-defense-and-quarantine, карантин посылок (quarantine.py/quarantine_lib.py + тесты _test_quarantine_sig25.py, _test_quarantine_tier2_gate.py, launchd-сторож), подписи Ed25519 на грантах [[sig-unify-ed25519-grants]], и прямо по букве отчёта — injection_detector остался ЛОГОМ, а замком стал детерминированный block_dangerous_bash.py (exit 2), память [[injection-detector-is-log-not-door]]. Синхронные грабли Syncthing-как-очереди (sync-conflict, частичная доставка, дубль-исполнение) тоже вошли в канон: манифест PENDING-<host> + verify перед apply (§7.5))
- started: 2026-06-25
- models asked: 1 · reports delivered: 0 · used in synthesis: not recorded

| model | report arrived |
|---|---|


Reports below are published as delivered, after an automatic privacy scrub.

- [head](./head.md)
