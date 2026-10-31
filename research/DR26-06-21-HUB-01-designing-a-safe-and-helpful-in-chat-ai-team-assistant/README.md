# How DR26-06-21-HUB-01 was run

- topic: Designing a Safe and Helpful In-Chat AI Team Assistant (Не берём: рекомендации совпали с уже действующим каноном и нового не порождают — тир-модель Read/Prepare/Propose/Execute = наши Tier-0/1/2 (§3.1); 4 режима поведения (act-and-report / offer-and-ask / ask-before-acting / stay-silent) = §4.1 батч-мандат + §3.1; детерминированный control plane вокруг LLM = approval.py + onair.py + гейты; 'approval theater' (человек одобряет пересказ, а не само действие) = наш замер draft-marked-waiting-is-not-an-ask. Причинность не проверялась — фиксирую совпадение, не влияние. Решение изменит: если начнём строить бота-ассистента с ИСПОЛНЯЮЩИМИ внешними действиями (закупки, платежи, отправка от лица команды) — тогда вернуться за tiered-governance, approval-mediator и 4-слойной памятью (knowledge/task/commercial/preference).)
- started: 2026-06-21
- models asked: 1 · reports delivered: 0 · used in synthesis: not recorded

| model | report arrived |
|---|---|


- prompt: not preserved (this research ran before we kept a prompt registry)

Reports below are published as delivered, after an automatic privacy scrub.

- [team](./team.md)
