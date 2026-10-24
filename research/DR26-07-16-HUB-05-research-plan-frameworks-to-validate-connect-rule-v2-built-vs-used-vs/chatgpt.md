# DR26-07-16-HUB-05 — raw report from chatgpt

> Unedited output of one model. Our reconciled synthesis is the page one level up.

---
title: "Anchor Node Resilience"
type: ai-conversation
stage: raw
source: chatgpt


authored_by: hybrid
conversation_id: 6a596230-40b8-83eb-a5d5-84cf97da2a2a
model: "gpt-5-6-pro"
url: "https://приватный чат"
date_recorded: 2026-07-17
date_added: 2026-07-19
valid_as_of: 2026-07-17
archived: false
volatility: slow
interpretation_confidence: high
intent: ""
language: ru
tags: [chatgpt, ai-conversation]
value_score: 0.0
msg_count: 19
related_concepts: []
revisit_if: ""
---

# Anchor Node Resilience

> ChatGPT · 2026-07-17 · `gpt-5-6-pro` · 19 сообщений

## Диалог

**Anton:**

DR26-07-16-HUB-05 — Operational resilience & failure-mode hardening of an always-on anchor node

Context. A small trusted fleet (4–6 machines, each running an LLM coding-agent) coordinates every change through an append-only, single-writer consensus ledger. One member is an always-on Linux VPS "anchor" (a single Hetzner Cloud instance) that carries three fleet duties: (1) tie-break arbiter for auto-resolving timed-out reversible proposals, (2) fold-node that merges single-writer shards into canonical files, (3) primary outbound (Telegram outreach). The other machines are personal computers that sleep, travel, and lose network. Leadership recently became roaming: the arbiter is the first machine in a fixed priority list whose presence heartbeat is fresh, and a human ("Anton") holds a super-crown whenever he is typing in a live session; canon/irreversible/money changes require his live presence.

What actually went wrong (the trigger). The anchor went silently dark because a Hetzner invoice went unpaid and the host suspended the instance. Operators, seeing "node unreachable," issued two reboots via the provider API before diagnosing the billing root cause — reboots that could never have worked, because a suspended-for-non-payment instance does not boot. The incident was noticed late and wasted effort on the wrong fix.

The question. How do we make a single always-on hosted anchor operationally bulletproof against the failure modes that actually hit us — billing/payment death, wrong triage order, and anchor-duty loss — while staying AK-47: a few hundred lines of stdlib Python + config, deterministic, human-auditable, no heavy orchestration platform (no Kubernetes, no Raft daemon), runnable by a non-technical owner?

Specifically address:
1. Billing / payment as a first-class failure mode. Monitor invoice, payment, and suspension state via a cloud provider's API (Hetzner Cloud API specifically, but generalize) and alert BEFORE suspension, not after. What signals exist and how early they fire (invoice issued, payment failed, grace period, suspension). A payment dead-man's-switch. Where the honest limit is.
2. Deterministic dead-node triage order. A runbook/decision-tree for "anchor unreachable" that checks in the correct order — account/billing status → cloud-API power state → network reachability → SSH/daemon liveness → only then a reboot — so we never again reboot a node that is suspended, out of disk, or OOM. Encode as a small script that emits the diagnosis, not a guess.
3. Anchor-duty failover / failback without split-brain. When the anchor is genuinely down, how do its three duties (arbiter, fold-node, outbound-primary) hand off to a warm standby (the always-on desktop hub) and hand back cleanly on recovery — given a roaming-arbiter election already exists? Address duplicate-effect avoidance, epoch/lease fencing, and the failback race.
4. Single-provider single-instance risk. The anchor is one VPS at one host = a single point of failure for billing, hardware, and region/maintenance outages. Evaluate a warm-standby-anchor doctrine: cost/complexity of a second cheap always-on node (different provider) vs simply letting the desktop hub cover; identity/DNS/secret-distribution implications; when the extra node is worth it for a personal fleet (not enterprise).
5. Hosted-anchor failure-mode catalogue. Beyond billing: silent host suspension, clock drift, disk-full, OOM-killed daemon, provider scheduled maintenance, IP change. For each: cheapest reliable detection signal, and whether the right response is auto-heal or alert-a-human.
6. LLM-agent-specific pitfalls. The "operator" diagnosing the dead anchor is itself an LLM agent that can hallucinate a cause or act on a stale status file. How to make the triage evidence-forced (must cite a live API/probe result before recommending an action).

Deliverable. Ranked, concrete hardening steps (high-value / low-effort first), each tagged with: the failure it closes, the minimal AK-47 mechanism, and whether it is config-only vs code vs a second node. Confidence-tag each (established / emerging / speculative). Explicitly flag where the honest limit is — what a personal fleet should NOT try to solve and should just accept + monitor. Search the web extensively and cite sources (provider docs, SRE runbooks, dead-man patterns).

---

**ChatGPT:** · `gpt-5-6-pro`

I’ll verify Hetzner’s actual billing signals and automation limits, then build deterministic triage and lease/epoch-fenced failover for all three anchor duties.

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:docs.hetzner.com cloud billing invoice payment failed suspension Hetzner Cloud"},{"q":"site:docs.hetzner.com cloud API billing invoices Hetzner"},{"q":"site:docs.hetzner.com account invoice due payment reminder suspended server"},{"q":"site:docs.hetzner.com cloud servers suspended non payment reboot"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:status.hetzner.com API RSS Atom maintenance Hetzner status"},{"q":"site:docs.hetzner.com Hetzner status scheduled maintenance notifications"},{"q":"site:status.hetzner.com \"maintenance\" cloud servers"},{"q":"site:docs.hetzner.com cloud scheduled maintenance email notification"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:core.telegram.org/bots/api sendMessage idempotency key duplicate"},{"q":"site:core.telegram.org/bots/api \"sendMessage\" \"message_thread_id\""},{"q":"site:core.telegram.org/bots/api \"update_id\" duplicates webhook"},{"q":"site:core.telegram.org/bots/api \"getUpdates\" duplicate"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"Gray Cheriton leases efficient fault-tolerant distributed file cache consistency PDF"},{"q":"Google Chubby lock service sequencer fencing token paper PDF"},{"q":"site:research.google/pubs chubby lock service distributed systems"},{"q":"site:static.googleusercontent.com chubby lock service paper sequencer"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

The key hard limit is clear: Hetzner’s Cloud API lacks invoice/payment status, so prevention must rely on billing email/account evidence plus independent payment acknowledgment.

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:docs.hetzner.cloud reference cloud servers get a server response locked public_net blocked"},{"q":"site:docs.hetzner.cloud \"public_net\" \"blocked\" server Hetzner Cloud API"},{"q":"site:docs.hetzner.cloud \"Get a Server\" \"locked\""}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
bash -lc cat > /mnt/data/anchor_guard.py <<'PY'
#!/usr/bin/env python3
"""AK-47 diagnostics for a Hetzner-hosted fleet anchor.

Read-only by design. It gathers evidence and emits a deterministic JSON diagnosis.
Subcommands:
  triage         Check billing evidence -> Hetzner API -> DNS/TCP -> SSH/local health.
  local-health   Run on the anchor (or through SSH) and emit guest health JSON.
  billing-scan   Scan a billing mailbox for provider messages; never infers "paid".
  billing-ack    Record a positive human/account check atomically.

Python stdlib only. It may call ubiquitous host tools: ssh, systemctl, chronyc,
timedatectl. No command in this file changes cloud power state.
"""
from __future__ import annotations

import argparse
import datetime as dt
import email
import email.policy
import hashlib
import imaplib
import json
import os
import re
import shlex
import socket
import ssl
import subprocess
import sys
import tempfile
import time
import urllib.error
import urllib.request
from pathlib import Path
from typing import Any, Iterable

UTC = dt.timezone.utc
VERSION = "1.0"

RISK_ORDER = {
    "clear": 0,
    "invoice_issued": 1,
    "payment_failed": 3,
    "overdue": 4,
    "suspension_warning": 5,
    "suspended": 6,
    "termination": 7,
    "unknown": 8,
}

DEFAULT_MAIL_PATTERNS: dict[str, list[str]] = {
    "termination": [
        r"notice of termination", r"terminate(?:d|ion)", r"kündig(?:ung|en)",
    ],
    "suspended": [
        r"account (?:has been )?(?:deactivated|suspended|locked)",
        r"service (?:has been )?(?:suspended|locked)",
        r"server (?:has been )?(?:suspended|locked)",
        r"konto (?:wurde )?(?:deaktiviert|gesperrt)",
    ],
    "suspension_warning": [
        r"will be (?:deactivated|suspended|locked)",
        r"risk of (?:deactivation|suspension)",
        r"deactivation warning", r"suspension warning",
        r"wird (?:deaktiviert|gesperrt)",
    ],
    "payment_failed": [
        r"payment (?:has )?failed", r"automatic charge (?:has )?failed",
        r"card (?:charge|payment) (?:has )?failed",
        r"zahlung (?:ist )?fehlgeschlagen", r"abbuchung .*fehlgeschlagen",
    ],
    "overdue": [
        r"payment reminder", r"past due", r"overdue invoice", r"invoice .*overdue",
        r"mahnung", r"zahlungs(?:erinnerung|aufforderung)",
    ],
    "invoice_issued": [
        r"\binvoice\b", r"\brechnung\b",
    ],
}


def utcnow() -> dt.datetime:
    return dt.datetime.now(UTC)


def iso(ts: dt.datetime | None = None) -> str:
    return (ts or utcnow()).astimezone(UTC).isoformat().replace("+00:00", "Z")


def parse_time(value: Any) -> dt.datetime | None:
    if not isinstance(value, str) or not value.strip():
        return None
    text = value.strip().replace("Z", "+00:00")
    try:
        parsed = dt.datetime.fromisoformat(text)
    except ValueError:
        try:
            parsed = email.utils.parsedate_to_datetime(value)
        except (TypeError, ValueError):
            return None
    if parsed.tzinfo is None:
        parsed = parsed.replace(tzinfo=UTC)
    return parsed.astimezone(UTC)


def age_seconds(value: Any) -> float | None:
    parsed = parse_time(value)
    return None if parsed is None else max(0.0, (utcnow() - parsed).total_seconds())


def load_json(path: str | Path, default: Any = None) -> Any:
    try:
        with open(path, "r", encoding="utf-8") as fh:
            return json.load(fh)
    except FileNotFoundError:
        return default
    except (OSError, json.JSONDecodeError) as exc:
        return {"_read_error": str(exc)}


def atomic_write_json(path: str | Path, value: Any, mode: int = 0o600) -> None:
    target = Path(path)
    target.parent.mkdir(parents=True, exist_ok=True)
    fd, tmp = tempfile.mkstemp(prefix=f".{target.name}.", dir=str(target.parent))
    try:
        os.fchmod(fd, mode)
        with os.fdopen(fd, "w", encoding="utf-8") as fh:
            json.dump(value, fh, indent=2, sort_keys=True)
            fh.write("\n")
            fh.flush()
            os.fsync(fh.fileno())
        os.replace(tmp, target)
        dirfd = os.open(target.parent, os.O_DIRECTORY)
        try:
            os.fsync(dirfd)
        finally:
            os.close(dirfd)
    finally:
        try:
            os.unlink(tmp)
        except FileNotFoundError:
            pass


def canonical_hash(value: Any) -> str:
    data = json.dumps(value, sort_keys=True, separators=(",", ":"), ensure_ascii=True)
    return hashlib.sha256(data.encode("utf-8")).hexdigest()


def run(cmd: list[str], timeout: float = 8.0) -> dict[str, Any]:
    started = time.monotonic()
    try:
        cp = subprocess.run(
            cmd,
            stdin=subprocess.DEVNULL,
            stdout=subprocess.PIPE,
            stderr=subprocess.PIPE,
            text=True,
            timeout=timeout,
            check=False,
        )
        return {
            "ok": cp.returncode == 0,
            "returncode": cp.returncode,
            "stdout": cp.stdout.strip(),
            "stderr": cp.stderr.strip(),
            "elapsed_ms": round((time.monotonic() - started) * 1000),
            "argv": cmd,
        }
    except FileNotFoundError as exc:
        return {"ok": False, "error": str(exc), "argv": cmd}
    except subprocess.TimeoutExpired as exc:
        return {
            "ok": False,
            "error": "timeout",
            "stdout": (exc.stdout or "").strip() if isinstance(exc.stdout, str) else "",
            "stderr": (exc.stderr or "").strip() if isinstance(exc.stderr, str) else "",
            "elapsed_ms": round((time.monotonic() - started) * 1000),
            "argv": cmd,
        }


def evidence(kind: str, ok: bool | None, value: Any, *, source: str,
             confidence: str = "direct") -> dict[str, Any]:
    return {
        "kind": kind,
        "observed_at": iso(),
        "ok": ok,
        "source": source,
        "confidence": confidence,
        "value": value,
    }


def read_config(path: str) -> dict[str, Any]:
    cfg = load_json(path)
    if not isinstance(cfg, dict) or cfg.get("_read_error"):
        raise SystemExit(f"cannot read config {path!r}: {cfg}")
    return cfg


# ------------------------------ billing ----------------------------------

def billing_state_file(cfg: dict[str, Any]) -> Path:
    return Path(cfg.get("billing", {}).get("state_file", "billing_state.json"))


def assess_billing(cfg: dict[str, Any]) -> tuple[dict[str, Any], dict[str, Any]]:
    bcfg = cfg.get("billing", {})
    state = load_json(billing_state_file(cfg), default={})
    if not isinstance(state, dict) or state.get("_read_error"):
        result = {
            "state": "unknown",
            "reason": "billing evidence missing or unreadable",
            "reboot_veto": True,
            "raw": state,
        }
        return result, evidence("billing", False, result, source="billing_state_file", confidence="direct")

    ack = state.get("account_ack") if isinstance(state.get("account_ack"), dict) else {}
    scan = state.get("mail_scan") if isinstance(state.get("mail_scan"), dict) else {}
    max_ack_h = float(bcfg.get("max_ack_age_hours", 168))
    max_scan_h = float(bcfg.get("max_scan_age_hours", 30))
    ack_age = age_seconds(ack.get("checked_at"))
    scan_age = age_seconds(scan.get("checked_at"))
    ack_fresh = ack_age is not None and ack_age <= max_ack_h * 3600
    scan_fresh = scan_age is not None and scan_age <= max_scan_h * 3600

    events = scan.get("events", []) if isinstance(scan.get("events"), list) else []
    trusted_events = [e for e in events if isinstance(e, dict) and e.get("trusted_sender")]
    latest_risk: dict[str, Any] | None = None
    for event in trusted_events:
        category = str(event.get("category", "unknown"))
        if RISK_ORDER.get(category, 8) < 3:
            continue
        if latest_risk is None:
            latest_risk = event
            continue
        a = parse_time(event.get("date")) or dt.datetime.min.replace(tzinfo=UTC)
        b = parse_time(latest_risk.get("date")) or dt.datetime.min.replace(tzinfo=UTC)
        if a > b or RISK_ORDER.get(category, 8) > RISK_ORDER.get(str(latest_risk.get("category")), 8):
            latest_risk = event

    ack_time = parse_time(ack.get("checked_at"))
    risk_time = parse_time(latest_risk.get("date")) if latest_risk else None
    risk_after_ack = latest_risk is not None and (ack_time is None or risk_time is None or risk_time > ack_time)

    if risk_after_ack:
        state_name = str(latest_risk.get("category", "unknown"))
        reason = f"trusted billing mail after last positive account check: {state_name}"
    elif not scan_fresh:
        state_name, reason = "unknown", "billing-mail scanner is stale or has never succeeded"
    elif not ack_fresh:
        state_name, reason = "unknown", "positive Hetzner Accounts check is stale or missing"
    elif ack.get("state") == "clear" and int(ack.get("open_invoices", -1)) == 0:
        state_name, reason = "clear", "fresh positive account check reports zero open invoices"
    else:
        state_name = str(ack.get("state", "unknown"))
        reason = "latest account check is not an explicit clear/zero-open-invoices assertion"

    result = {
        "state": state_name,
        "reason": reason,
        "reboot_veto": state_name != "clear",
        "account_ack": ack,
        "account_ack_age_seconds": ack_age,
        "mail_scan_checked_at": scan.get("checked_at"),
        "mail_scan_age_seconds": scan_age,
        "latest_risk_event": latest_risk,
        "limitation": "No public Hetzner Cloud invoice/payment-status endpoint is used; clear requires a positive account check.",
    }
    return result, evidence("billing", state_name == "clear", result,
                            source=str(billing_state_file(cfg)), confidence="mixed")


def extract_text(msg: email.message.EmailMessage, max_chars: int = 20000) -> str:
    chunks: list[str] = []
    if msg.is_multipart():
        for part in msg.walk():
            ctype = part.get_content_type()
            disp = (part.get("Content-Disposition") or "").lower()
            if "attachment" in disp or ctype not in {"text/plain", "text/html"}:
                continue
            try:
                content = part.get_content()
            except (LookupError, UnicodeDecodeError):
                payload = part.get_payload(decode=True) or b""
                content = payload.decode("utf-8", "replace")
            chunks.append(str(content))
    else:
        try:
            chunks.append(str(msg.get_content()))
        except (LookupError, UnicodeDecodeError):
            payload = msg.get_payload(decode=True) or b""
            chunks.append(payload.decode("utf-8", "replace"))
    text = "\n".join(chunks)
    text = re.sub(r"<[^>]+>", " ", text)
    return re.sub(r"\s+", " ", text)[:max_chars]


def sender_domain(msg: email.message.EmailMessage) -> str:
    addresses = email.utils.getaddresses(msg.get_all("From", []))
    if not addresses or "@" not in addresses[0][1]:
        return ""
    return addresses[0][1].rsplit("@", 1)[1].lower().strip(".>")


def sender_authenticated(msg: email.message.EmailMessage, domain: str) -> bool:
    auth = " ".join(msg.get_all("Authentication-Results", [])).lower()
    if not auth:
        return False
    dkim = "dkim=pass" in auth and (f"header.d={domain}" in auth or "header.d=hetzner.com" in auth)
    spf = "spf=pass" in auth and (domain in auth or "hetzner.com" in auth)
    dmarc = "dmarc=pass" in auth and (domain in auth or "hetzner.com" in auth)
    return dkim or dmarc or spf


def classify_mail(text: str, patterns: dict[str, list[str]]) -> str | None:
    for category in ("termination", "suspended", "suspension_warning", "payment_failed", "overdue", "invoice_issued"):
        for pattern in patterns.get(category, []):
            if re.search(pattern, text, flags=re.IGNORECASE | re.DOTALL):
                return category
    return None


def billing_scan(cfg: dict[str, Any]) -> dict[str, Any]:
    bcfg = cfg.get("billing", {})
    icfg = bcfg.get("imap", {})
    host = icfg.get("host")
    if not host:
        raise SystemExit("billing.imap.host is required")
    user = os.environ.get(str(icfg.get("username_env", "BILLING_IMAP_USER")), "")
    password = os.environ.get(str(icfg.get("password_env", "BILLING_IMAP_PASSWORD")), "")
    if not user or not password:
        raise SystemExit("billing IMAP username/password environment variables are missing")

    allowed = [str(x).lower().strip(".") for x in icfg.get("allowed_from_domains", ["hetzner.com"])]
    patterns = {k: list(v) for k, v in DEFAULT_MAIL_PATTERNS.items()}
    for key, values in icfg.get("patterns", {}).items():
        patterns[str(key)] = [str(x) for x in values]
    lookback = int(icfg.get("lookback_days", 45))
    since = (utcnow() - dt.timedelta(days=lookback)).strftime("%d-%b-%Y")
    max_messages = int(icfg.get("max_messages", 200))

    context = ssl.create_default_context()
    client = imaplib.IMAP4_SSL(host, int(icfg.get("port", 993)), ssl_context=context)
    events: list[dict[str, Any]] = []
    try:
        client.login(user, password)
        status, _ = client.select(str(icfg.get("mailbox", "INBOX")), readonly=True)
        if status != "OK":
            raise RuntimeError("cannot select mailbox")
        status, data = client.search(None, "SINCE", since)
        if status != "OK":
            raise RuntimeError("IMAP search failed")
        ids = (data[0] or b"").split()[-max_messages:]
        for message_id in ids:
            status, parts = client.fetch(message_id, "(RFC822)")
            if status != "OK" or not parts:
                continue
            raw = next((p[1] for p in parts if isinstance(p, tuple) and len(p) > 1), None)
            if not isinstance(raw, (bytes, bytearray)):
                continue
            msg = email.message_from_bytes(raw, policy=email.policy.default)
            domain = sender_domain(msg)
            domain_allowed = any(domain == d or domain.endswith("." + d) for d in allowed)
            if not domain_allowed:
                continue
            subject = str(msg.get("Subject", ""))
            body = extract_text(msg)
            category = classify_mail(subject + "\n" + body, patterns)
            if not category:
                continue
            msg_date = parse_time(str(msg.get("Date", ""))) or utcnow()
            auth_ok = sender_authenticated(msg, domain)
            events.append({
                "category": category,
                "date": iso(msg_date),
                "from": str(msg.get("From", "")),
                "sender_domain": domain,
                "subject": subject[:300],
                "message_id": str(msg.get("Message-ID", ""))[:300],
                "trusted_sender": bool(domain_allowed and auth_ok),
                "auth_results_present": bool(msg.get_all("Authentication-Results", [])),
            })
    finally:
        try:
            client.logout()
        except Exception:
            pass

    events.sort(key=lambda item: item.get("date", ""))
    state_path = billing_state_file(cfg)
    state = load_json(state_path, default={})
    if not isinstance(state, dict) or state.get("_read_error"):
        state = {}
    state["mail_scan"] = {
        "checked_at": iso(),
        "host": host,
        "mailbox": str(icfg.get("mailbox", "INBOX")),
        "lookback_days": lookback,
        "events": events,
        "note": "Absence of warning mail is not proof of payment.",
    }
    atomic_write_json(state_path, state)

    ping_env = bcfg.get("deadman_ping_url_env")
    ping_url = os.environ.get(str(ping_env), "") if ping_env else ""
    ping = None
    if ping_url:
        req = urllib.request.Request(ping_url, headers={"User-Agent": f"anchor-guard/{VERSION}"})
        try:
            with urllib.request.urlopen(req, timeout=10) as resp:
                ping = {"ok": 200 <= resp.status < 300, "status": resp.status}
        except Exception as exc:
            ping = {"ok": False, "error": str(exc)}

    return {"checked_at": iso(), "events": events, "deadman_ping": ping, "state_file": str(state_path)}


def billing_ack(cfg: dict[str, Any], args: argparse.Namespace) -> dict[str, Any]:
    if args.state == "clear" and args.open_invoices != 0:
        raise SystemExit("a clear acknowledgment requires --open-invoices 0")
    state_path = billing_state_file(cfg)
    state = load_json(state_path, default={})
    if not isinstance(state, dict) or state.get("_read_error"):
        state = {}
    ack = {
        "checked_at": iso(),
        "state": args.state,
        "open_invoices": args.open_invoices,
        "source": args.source,
        "operator": args.operator,
        "invoice_id": args.invoice_id,
        "note": args.note,
    }
    ack["ack_hash"] = canonical_hash(ack)
    state["account_ack"] = ack
    atomic_write_json(state_path, state)
    return ack


# --------------------------- provider/network -----------------------------

def hcloud_server(cfg: dict[str, Any]) -> tuple[dict[str, Any] | None, dict[str, Any]]:
    hcfg = cfg.get("hetzner", {})
    acfg = cfg.get("anchor", {})
    token = os.environ.get(str(hcfg.get("token_env", "HCLOUD_TOKEN_READ")), "")
    if not token:
        value = {"error": "missing read-only API token"}
        return None, evidence("hetzner_api", False, value, source="api.hetzner.cloud")
    server_id = acfg.get("server_id")
    base = str(hcfg.get("api_base", "https://api.hetzner.cloud/v1")).rstrip("/")
    url = f"{base}/servers/{server_id}"
    req = urllib.request.Request(
        url,
        headers={"Authorization": f"Bearer {token}", "User-Agent": f"anchor-guard/{VERSION}"},
        method="GET",
    )
    timeout = float(acfg.get("timeout_seconds", 4.0))
    started = time.monotonic()
    try:
        with urllib.request.urlopen(req, timeout=timeout) as resp:
            payload = json.loads(resp.read().decode("utf-8"))
            server = payload.get("server")
            if not isinstance(server, dict):
                raise ValueError("response has no server object")
            value = {
                "http_status": resp.status,
                "elapsed_ms": round((time.monotonic() - started) * 1000),
                "server": {
                    "id": server.get("id"),
                    "name": server.get("name"),
                    "status": server.get("status"),
                    "locked": server.get("locked"),
                    "ipv4": ((server.get("public_net") or {}).get("ipv4") or {}).get("ip"),
                    "ipv4_blocked": ((server.get("public_net") or {}).get("ipv4") or {}).get("blocked"),
                    "ipv6_blocked": ((server.get("public_net") or {}).get("ipv6") or {}).get("blocked"),
                    "rescue_enabled": server.get("rescue_enabled"),
                },
            }
            return server, evidence("hetzner_api", True, value, source=url)
    except urllib.error.HTTPError as exc:
        try:
            body = exc.read().decode("utf-8", "replace")[:2000]
        except Exception:
            body = ""
        value = {
            "http_status": exc.code,
            "reason": str(exc.reason),
            "body": body,
            "elapsed_ms": round((time.monotonic() - started) * 1000),
        }
        return None, evidence("hetzner_api", False, value, source=url)
    except Exception as exc:
        value = {"error": str(exc), "elapsed_ms": round((time.monotonic() - started) * 1000)}
        return None, evidence("hetzner_api", False, value, source=url)


def dns_probe(hostname: str, timeout: float) -> tuple[list[str], dict[str, Any]]:
    old = socket.getdefaulttimeout()
    socket.setdefaulttimeout(timeout)
    try:
        infos = socket.getaddrinfo(hostname, None, socket.AF_INET, socket.SOCK_STREAM)
        ips = sorted({item[4][0] for item in infos})
        return ips, evidence("dns", bool(ips), {"hostname": hostname, "ipv4": ips}, source="system resolver")
    except Exception as exc:
        return [], evidence("dns", False, {"hostname": hostname, "error": str(exc)}, source="system resolver")
    finally:
        socket.setdefaulttimeout(old)


def tcp_probe(host: str, port: int, timeout: float) -> dict[str, Any]:
    started = time.monotonic()
    try:
        with socket.create_connection((host, port), timeout=timeout):
            return evidence(
                f"tcp:{port}", True,
                {"host": host, "port": port, "elapsed_ms": round((time.monotonic() - started) * 1000)},
                source="socket.connect",
            )
    except Exception as exc:
        return evidence(
            f"tcp:{port}", False,
            {"host": host, "port": port, "error": str(exc), "elapsed_ms": round((time.monotonic() - started) * 1000)},
            source="socket.connect",
        )


def http_health_probe(acfg: dict[str, Any], host: str) -> dict[str, Any] | None:
    port = acfg.get("health_port")
    if not port:
        return None
    scheme = "https" if acfg.get("health_tls", True) else "http"
    path = str(acfg.get("health_path", "/healthz"))
    url = f"{scheme}://{host}:{int(port)}{path}"
    req = urllib.request.Request(url, headers={"User-Agent": f"anchor-guard/{VERSION}"}, method="GET")
    context = ssl.create_default_context()
    if acfg.get("health_tls_insecure", False):
        context.check_hostname = False
        context.verify_mode = ssl.CERT_NONE
    started = time.monotonic()
    try:
        with urllib.request.urlopen(req, timeout=float(acfg.get("timeout_seconds", 4.0)), context=context) as resp:
            body = resp.read(65536).decode("utf-8", "replace")
            parsed: Any = body
            try:
                parsed = json.loads(body)
            except json.JSONDecodeError:
                pass
            return evidence(
                "http_health", 200 <= resp.status < 300,
                {"url": url, "status": resp.status, "body": parsed,
                 "elapsed_ms": round((time.monotonic() - started) * 1000)},
                source=url,
            )
    except Exception as exc:
        return evidence(
            "http_health", False,
            {"url": url, "error": str(exc), "elapsed_ms": round((time.monotonic() - started) * 1000)},
            source=url,
        )


def ssh_health(cfg: dict[str, Any]) -> dict[str, Any] | None:
    acfg = cfg.get("anchor", {})
    target = acfg.get("ssh_target")
    if not target:
        return None
    timeout = int(float(acfg.get("timeout_seconds", 4.0)))
    remote = acfg.get("ssh_health_command", ["/usr/local/sbin/anchor_guard.py", "local-health", "--config", "/etc/anchor-guard.json"])
    if isinstance(remote, str):
        remote_args = shlex.split(remote)
    else:
        remote_args = [str(x) for x in remote]
    cmd = [
        "ssh", "-o", "BatchMode=yes", "-o", f"ConnectTimeout={max(1, timeout)}",
        "-o", "ConnectionAttempts=1", "-o", "PasswordAuthentication=no",
        "-o", "NumberOfPasswordPrompts=0", "-o", "ControlMaster=no",
        "-p", str(int(acfg.get("ssh_port", 22))), str(target), *remote_args,
    ]
    result = run(cmd, timeout=max(6.0, timeout + 4.0))
    parsed: Any = result.get("stdout", "")
    if result.get("ok"):
        try:
            parsed = json.loads(str(result.get("stdout", "")))
        except json.JSONDecodeError:
            result["ok"] = False
            result["parse_error"] = "remote health output was not JSON"
    result["parsed"] = parsed
    # Do not expose the full SSH argv if it contains unusual config; target is sufficient.
    result.pop("argv", None)
    return evidence("ssh_local_health", bool(result.get("ok")), result, source=f"ssh:{target}")


def oob_evidence(cfg: dict[str, Any]) -> dict[str, Any]:
    path = cfg.get("anchor", {}).get("oob_evidence_file")
    if not path:
        return evidence("out_of_band", None, {"state": "not_configured"}, source="none", confidence="none")
    value = load_json(path, default={})
    max_age_h = float(cfg.get("anchor", {}).get("oob_max_age_hours", 1))
    age = age_seconds(value.get("checked_at")) if isinstance(value, dict) else None
    fresh = age is not None and age <= max_age_h * 3600
    return evidence("out_of_band", fresh, {"fresh": fresh, "age_seconds": age, "data": value}, source=str(path))


# ----------------------------- local health -------------------------------

def statvfs_health(path: str) -> dict[str, Any]:
    try:
        st = os.statvfs(path)
        total = st.f_frsize * st.f_blocks
        free = st.f_frsize * st.f_bavail
        inode_total = st.f_files
        inode_free = st.f_favail
        return {
            "ok": True,
            "path": path,
            "total_bytes": total,
            "free_bytes": free,
            "free_percent": (100.0 * free / total) if total else None,
            "inode_total": inode_total,
            "inode_free": inode_free,
            "inode_free_percent": (100.0 * inode_free / inode_total) if inode_total else None,
        }
    except OSError as exc:
        return {"ok": False, "path": path, "error": str(exc)}


def fsync_probe(path: str) -> dict[str, Any]:
    started = time.monotonic()
    try:
        fd, name = tempfile.mkstemp(prefix=".anchor-write-probe.", dir=path)
        try:
            os.write(fd, b"anchor-guard\n")
            os.fsync(fd)
        finally:
            os.close(fd)
            try:
                os.unlink(name)
            except FileNotFoundError:
                pass
        return {"ok": True, "elapsed_ms": round((time.monotonic() - started) * 1000)}
    except OSError as exc:
        return {"ok": False, "error": str(exc), "elapsed_ms": round((time.monotonic() - started) * 1000)}


def parse_key_value_lines(text: str, sep: str = "=") -> dict[str, str]:
    out: dict[str, str] = {}
    for line in text.splitlines():
        if sep in line:
            key, value = line.split(sep, 1)
            out[key.strip()] = value.strip()
    return out


def systemd_health(unit: str) -> dict[str, Any]:
    result = run([
        "systemctl", "show", unit,
        "--property=ActiveState,SubState,Result,NRestarts,ExecMainStatus,ExecMainCode,ControlGroup",
    ])
    if not result.get("ok"):
        return result
    values = parse_key_value_lines(str(result.get("stdout", "")))
    result["properties"] = values
    result["ok"] = values.get("ActiveState") == "active"
    return result


def memory_events(control_group: str | None) -> dict[str, Any]:
    if not control_group:
        return {"ok": False, "error": "no ControlGroup from systemd"}
    path = Path("/sys/fs/cgroup") / control_group.lstrip("/") / "memory.events"
    try:
        values: dict[str, int] = {}
        for line in path.read_text(encoding="utf-8").splitlines():
            key, value = line.split(None, 1)
            values[key] = int(value)
        return {"ok": True, "path": str(path), "values": values}
    except (OSError, ValueError) as exc:
        return {"ok": False, "path": str(path), "error": str(exc)}


def read_pressure(resource: str) -> dict[str, Any]:
    path = Path("/proc/pressure") / resource
    try:
        lines: dict[str, dict[str, float]] = {}
        for line in path.read_text(encoding="utf-8").splitlines():
            parts = line.split()
            metrics: dict[str, float] = {}
            for item in parts[1:]:
                key, value = item.split("=", 1)
                metrics[key] = float(value)
            lines[parts[0]] = metrics
        return {"ok": True, "path": str(path), "metrics": lines}
    except (OSError, ValueError) as exc:
        return {"ok": False, "path": str(path), "error": str(exc)}


def clock_health() -> dict[str, Any]:
    chrony = run(["chronyc", "tracking"], timeout=4)
    if chrony.get("ok"):
        text = str(chrony.get("stdout", ""))
        fields: dict[str, str] = {}
        for line in text.splitlines():
            if ":" in line:
                k, v = line.split(":", 1)
                fields[k.strip()] = v.strip()
        offset = None
        m = re.search(r"System time\s*:\s*([0-9.]+)\s+seconds\s+(fast|slow)", text, re.I)
        if m:
            offset = float(m.group(1)) * (1 if m.group(2).lower() == "fast" else -1)
        leap = fields.get("Leap status", "")
        return {
            "ok": leap.lower() not in {"not synchronised", "not synchronized"},
            "source": "chronyc tracking",
            "offset_seconds": offset,
            "leap_status": leap,
            "fields": fields,
        }
    timedate = run(["timedatectl", "show", "--property=NTPSynchronized", "--value"], timeout=4)
    if timedate.get("ok"):
        synced = str(timedate.get("stdout", "")).strip().lower() == "yes"
        return {"ok": synced, "source": "timedatectl", "ntp_synchronized": synced, "offset_seconds": None}
    return {"ok": False, "source": "none", "chronyc": chrony, "timedatectl": timedate}


def heartbeat_health(path: str | None, max_age_s: float) -> dict[str, Any]:
    if not path:
        return {"ok": None, "state": "not_configured"}
    try:
        st = os.stat(path)
        age = max(0.0, time.time() - st.st_mtime)
        content = load_json(path, default=None)
        return {"ok": age <= max_age_s, "path": path, "age_seconds": age, "content": content}
    except OSError as exc:
        return {"ok": False, "path": path, "error": str(exc)}


def local_health(cfg: dict[str, Any]) -> dict[str, Any]:
    lcfg = cfg.get("local_health", {})
    state_dir = str(lcfg.get("state_dir", "/var/lib/Маяк"))
    unit = str(lcfg.get("systemd_unit", "Маяк.service"))
    disk = statvfs_health(state_dir)
    write = fsync_probe(state_dir) if disk.get("ok") else {"ok": False, "error": "statvfs failed"}
    service = systemd_health(unit)
    props = service.get("properties", {}) if isinstance(service.get("properties"), dict) else {}
    mem_events = memory_events(props.get("ControlGroup"))
    clock = clock_health()
    heartbeat = heartbeat_health(lcfg.get("heartbeat_file"), float(lcfg.get("heartbeat_max_age_seconds", 90)))
    pressure = {name: read_pressure(name) for name in ("memory", "io")}

    min_free_bytes = int(lcfg.get("min_free_bytes", 1_073_741_824))
    min_free_pct = float(lcfg.get("min_free_percent", 10))
    min_inode_pct = float(lcfg.get("min_inode_free_percent", 10))
    max_clock = float(lcfg.get("max_clock_error_seconds", 2))

    disk_ok = bool(disk.get("ok")) and int(disk.get("free_bytes", 0)) >= min_free_bytes
    if disk.get("free_percent") is not None:
        disk_ok = disk_ok and float(disk["free_percent"]) >= min_free_pct
    if disk.get("inode_free_percent") is not None:
        disk_ok = disk_ok and float(disk["inode_free_percent"]) >= min_inode_pct
    clock_ok = bool(clock.get("ok"))
    if clock.get("offset_seconds") is not None:
        clock_ok = clock_ok and abs(float(clock["offset_seconds"])) <= max_clock
    service_ok = bool(service.get("ok"))
    heartbeat_ok = heartbeat.get("ok") is not False
    oom_kills = ((mem_events.get("values") or {}).get("oom_kill", 0)
                 if isinstance(mem_events.get("values"), dict) else None)

    result = {
        "schema": "anchor-local-health/v1",
        "checked_at": iso(),
        "boot_id": Path("/proc/sys/kernel/random/boot_id").read_text().strip()
        if Path("/proc/sys/kernel/random/boot_id").exists() else None,
        "disk": disk,
        "write_fsync": write,
        "service": service,
        "memory_events": mem_events,
        "pressure": pressure,
        "clock": clock,
        "heartbeat": heartbeat,
        "summary": {
            "disk_ok": disk_ok,
            "write_ok": bool(write.get("ok")),
            "service_ok": service_ok,
            "heartbeat_ok": heartbeat_ok,
            "clock_ok": clock_ok,
            "oom_kill_count_since_cgroup_creation": oom_kills,
        },
    }
    result["ok"] = all([
        result["summary"]["disk_ok"], result["summary"]["write_ok"],
        result["summary"]["service_ok"], result["summary"]["heartbeat_ok"],
        result["summary"]["clock_ok"],
    ])
    return result


# ------------------------------- diagnosis --------------------------------

def value_of(evidences: Iterable[dict[str, Any]], kind: str) -> dict[str, Any] | None:
    return next((x for x in evidences if x.get("kind") == kind), None)


def classify(cfg: dict[str, Any], bill: dict[str, Any], server: dict[str, Any] | None,
             evidences: list[dict[str, Any]]) -> dict[str, Any]:
    acfg = cfg.get("anchor", {})
    api_ev = value_of(evidences, "hetzner_api") or {}
    dns_ev = value_of(evidences, "dns") or {}
    ssh_ev = value_of(evidences, "ssh_local_health")
    http_ev = value_of(evidences, "http_health")
    oob_ev = value_of(evidences, "out_of_band") or {}
    ssh_tcp = value_of(evidences, f"tcp:{int(acfg.get('ssh_port', 22))}") or {}
    health_port = acfg.get("health_port")
    health_tcp = value_of(evidences, f"tcp:{int(health_port)}") if health_port else None

    code = "UNKNOWN"
    severity = "warning"
    explanation = "Evidence is insufficient for a safe diagnosis."
    actions: list[str] = []
    vetoes: list[str] = []

    if bill.get("state") != "clear":
        code = "BILLING_RISK" if bill.get("state") not in {"unknown", "invoice_issued"} else "BILLING_UNKNOWN"
        severity = "critical" if code == "BILLING_RISK" else "warning"
        explanation = bill.get("reason", "billing not clear")
        actions = ["Open Hetzner Accounts -> Invoices and establish a fresh positive account check.",
                   "Do not issue power mutations while billing/account state is risky or unknown."]
        vetoes.append("billing_not_fresh_clear")

    if not api_ev.get("ok"):
        status = ((api_ev.get("value") or {}).get("http_status")
                  if isinstance(api_ev.get("value"), dict) else None)
        if code in {"UNKNOWN", "BILLING_UNKNOWN"}:
            code = "PROVIDER_API_AUTH_OR_CONTROL_PLANE" if status in {401, 403} else "PROVIDER_API_UNAVAILABLE"
            severity = "critical" if status in {401, 403} else "warning"
            explanation = "Hetzner control-plane evidence is unavailable; power state cannot be established."
            actions = ["Verify the read-only API token/project and inspect Hetzner status/account UI."]
        vetoes.append("provider_api_not_live")

    if server is not None:
        status = str(server.get("status", "unknown"))
        locked = bool(server.get("locked"))
        public = server.get("public_net") or {}
        ip4 = public.get("ipv4") or {}
        ip6 = public.get("ipv6") or {}
        blocked = bool(ip4.get("blocked")) or bool(ip6.get("blocked"))

        if blocked:
            code, severity = "PROVIDER_NETWORK_BLOCK", "critical"
            explanation = "Hetzner reports a public IP as blocked; reboot cannot remove a provider-side block."
            actions = ["Inspect provider email/account/support evidence and traceroute; do not reboot."]
            vetoes.append("provider_ip_blocked")
        elif locked:
            code, severity = "PROVIDER_RESOURCE_LOCK", "warning"
            explanation = "Hetzner reports the server resource locked, commonly while a provider action is active."
            actions = ["Inspect current/recent cloud actions and wait or contact support; do not stack another action."]
            vetoes.append("provider_resource_locked")
        elif status == "off":
            code, severity = "POWERED_OFF", "critical"
            explanation = "The provider reports the VM powered off. This calls for power-on, not reboot."
            actions = ["After billing is freshly clear, issue one power-on and observe the action result."]
            vetoes.append("server_not_running")
        elif status in {"initializing", "starting", "stopping", "migrating", "rebuilding", "deleting"}:
            code, severity = "PROVIDER_TRANSITION", "warning"
            explanation = f"The provider reports a transitional state: {status}."
            actions = ["Wait for the current action to settle and inspect its result before any new mutation."]
            vetoes.append("server_in_transition")
        elif status != "running":
            code, severity = "PROVIDER_STATE_UNKNOWN", "warning"
            explanation = f"The provider state is {status!r}, not a confirmed running VM."
            actions = ["Inspect provider actions/status/support before any reboot."]
            vetoes.append("provider_state_not_running")
        else:
            api_ip = (ip4.get("ip") or "").strip()
            dns_ips = ((dns_ev.get("value") or {}).get("ipv4", [])
                       if isinstance(dns_ev.get("value"), dict) else [])
            expected_ip = str(acfg.get("expected_ipv4", "")).strip()
            mismatch = bool(api_ip and dns_ips and api_ip not in dns_ips)
            mismatch = mismatch or bool(expected_ip and api_ip and expected_ip != api_ip)
            if mismatch:
                code, severity = "DNS_OR_IP_DRIFT", "critical"
                explanation = "Provider IP, configured identity, and DNS do not agree."
                actions = ["Correct or explicitly authorize the IP/DNS change; do not reboot as a DNS repair."]
                vetoes.append("identity_mismatch")
            elif ssh_ev and ssh_ev.get("ok"):
                parsed = ((ssh_ev.get("value") or {}).get("parsed")
                          if isinstance(ssh_ev.get("value"), dict) else None)
                summary = parsed.get("summary", {}) if isinstance(parsed, dict) else {}
                if summary.get("disk_ok") is False or summary.get("write_ok") is False:
                    code, severity = "DISK_OR_FILESYSTEM_PRESSURE", "critical"
                    explanation = "Guest evidence reports low disk/inodes or a failed write+fsync probe."
                    actions = ["Stop writers/fold duties, free only known disposable data, then verify ledger integrity."]
                    vetoes.append("disk_or_filesystem_unhealthy")
                elif summary.get("clock_ok") is False:
                    code, severity = "CLOCK_UNSAFE", "critical"
                    explanation = "Guest clock evidence is outside the configured safety bound or unsynchronized."
                    actions = ["Self-fence all leased duties, repair time synchronization, then reacquire a new epoch."]
                    vetoes.append("clock_unsafe")
                elif summary.get("service_ok") is False or summary.get("heartbeat_ok") is False:
                    code, severity = "DAEMON_FAILED_OR_HUNG", "critical"
                    explanation = "The VM and SSH are alive, but the anchor daemon or heartbeat is unhealthy."
                    actions = ["Inspect service result/OOM evidence and restart only the daemon, not the VM."]
                    vetoes.append("guest_daemon_unhealthy")
                elif isinstance(parsed, dict) and parsed.get("ok"):
                    code, severity = "ANCHOR_HEALTHY", "info"
                    explanation = "Provider, network, SSH, disk, daemon, heartbeat, and clock checks are healthy."
                    actions = ["Do not reboot; investigate the caller path or stale monitoring evidence."]
                else:
                    code, severity = "GUEST_HEALTH_AMBIGUOUS", "warning"
                    explanation = "SSH works, but the guest health result is incomplete or degraded."
                    actions = ["Use the reported guest evidence to repair the narrow fault; do not reboot by default."]
                    vetoes.append("guest_health_not_clear")
            elif http_ev and http_ev.get("ok"):
                code, severity = "SSH_PATH_FAILED_SERVICE_ALIVE", "warning"
                explanation = "The service health endpoint is live while SSH is not; this is not a dead VM."
                actions = ["Repair SSH/firewall/access separately; do not reboot the service host."]
                vetoes.append("service_still_alive")
            elif ssh_tcp.get("ok") or (health_tcp and health_tcp.get("ok")):
                code, severity = "PORT_OPEN_APPLICATION_CHECK_FAILED", "warning"
                explanation = "At least one TCP listener is reachable, so the VM/network is not wholly dead."
                actions = ["Inspect protocol/TLS/authentication or daemon state; do not reboot yet."]
                vetoes.append("network_not_dead")
            else:
                code, severity = "NETWORK_OR_GUEST_HANG", "critical"
                explanation = "Provider says running, but DNS-correct SSH and health probes are unreachable."
                actions = [
                    "Check official provider status and the out-of-band console/rescue view.",
                    "Reboot only after fresh console evidence identifies a reboot-safe guest hang or kernel panic.",
                ]
                vetoes.append("no_console_diagnosis")

    # Reboot is intentionally a very narrow, evidence-gated candidate, never an action here.
    oob_data = ((oob_ev.get("value") or {}).get("data", {})
                if isinstance(oob_ev.get("value"), dict) else {})
    oob_fresh = bool((oob_ev.get("value") or {}).get("fresh")) if isinstance(oob_ev.get("value"), dict) else False
    reboot_candidate = (
        code == "NETWORK_OR_GUEST_HANG"
        and bill.get("state") == "clear"
        and server is not None and server.get("status") == "running"
        and oob_fresh
        and oob_data.get("provider_status") == "clear"
        and oob_data.get("console_state") in {"guest_hung_reboot_safe", "kernel_panic_reboot_safe"}
    )
    if reboot_candidate:
        vetoes = [v for v in vetoes if v != "no_console_diagnosis"]
        actions.append("A single controlled reboot is now a candidate; record the evidence hash and observe the API action.")

    return {
        "code": code,
        "severity": severity,
        "explanation": explanation,
        "recommended_actions": actions,
        "reboot_candidate": reboot_candidate,
        "reboot_vetoes": sorted(set(vetoes + (["billing_not_fresh_clear"] if bill.get("state") != "clear" else []))),
        "policy": "Read-only diagnosis. No provider mutation is performed.",
    }


def triage(cfg: dict[str, Any]) -> dict[str, Any]:
    acfg = cfg.get("anchor", {})
    timeout = float(acfg.get("timeout_seconds", 4.0))
    hostname = str(acfg.get("hostname", ""))
    evidences: list[dict[str, Any]] = []

    bill, bill_ev = assess_billing(cfg)
    evidences.append(bill_ev)

    server, api_ev = hcloud_server(cfg)
    evidences.append(api_ev)

    dns_ips: list[str] = []
    if hostname:
        dns_ips, dns_ev = dns_probe(hostname, timeout)
        evidences.append(dns_ev)

    api_ip = ""
    if server:
        api_ip = str((((server.get("public_net") or {}).get("ipv4") or {}).get("ip") or ""))
    probe_host = hostname or api_ip
    if probe_host:
        evidences.append(tcp_probe(probe_host, int(acfg.get("ssh_port", 22)), timeout))
        if acfg.get("health_port"):
            evidences.append(tcp_probe(probe_host, int(acfg["health_port"]), timeout))
            hp = http_health_probe(acfg, probe_host)
            if hp:
                evidences.append(hp)

    ssh_tcp_ev = value_of(evidences, f"tcp:{int(acfg.get('ssh_port', 22))}")
    if ssh_tcp_ev and ssh_tcp_ev.get("ok"):
        shp = ssh_health(cfg)
        if shp:
            evidences.append(shp)

    evidences.append(oob_evidence(cfg))
    decision = classify(cfg, bill, server, evidences)
    output = {
        "schema": "anchor-triage/v1",
        "tool_version": VERSION,
        "checked_at": iso(),
        "anchor": {"name": acfg.get("name"), "server_id": acfg.get("server_id"), "hostname": hostname},
        "decision": decision,
        "evidence": evidences,
        "evidence_freshness_rule": "Only live probes and explicitly timestamped files are used; stale/missing data becomes UNKNOWN.",
    }
    output["diagnosis_hash"] = canonical_hash(output)
    return output


def make_parser() -> argparse.ArgumentParser:
    p = argparse.ArgumentParser(description=__doc__)
    p.add_argument("--config", default="anchor-guard.json", help="JSON configuration file")
    sub = p.add_subparsers(dest="command", required=True)
    sub.add_parser("triage", help="emit deterministic read-only diagnosis")
    sub.add_parser("local-health", help="emit local guest health")
    sub.add_parser("billing-scan", help="scan billing mailbox and update evidence")
    ack = sub.add_parser("billing-ack", help="record positive/negative human account evidence")
    ack.add_argument("--state", choices=["clear", "invoice_issued", "payment_failed", "overdue", "suspension_warning", "suspended", "termination", "unknown"], required=True)
    ack.add_argument("--open-invoices", type=int, required=True)
    ack.add_argument("--source", default="hetzner-accounts-human")
    ack.add_argument("--operator", required=True)
    ack.add_argument("--invoice-id", default="")
    ack.add_argument("--note", default="")
    return p


def main() -> int:
    args = make_parser().parse_args()
    cfg = read_config(args.config)
    if args.command == "triage":
        result = triage(cfg)
    elif args.command == "local-health":
        result = local_health(cfg)
    elif args.command == "billing-scan":
        result = billing_scan(cfg)
    elif args.command == "billing-ack":
        result = billing_ack(cfg, args)
    else:
        raise AssertionError(args.command)
    json.dump(result, sys.stdout, indent=2, sort_keys=True)
    sys.stdout.write("\n")
    if args.command == "triage":
        severity = result.get("decision", {}).get("severity")
        return 0 if severity == "info" else 2
    if args.command == "local-health":
        return 0 if result.get("ok") else 2
    return 0


if __name__ == "__main__":
    raise SystemExit(main())
PY

cat > /mnt/data/anchor-guard.example.json <<'JSON'
{
  "anchor": {
    "name": "anchor-hz1",
    "server_id": 12345678,
    "hostname": "anchor.example.net",
    "expected_ipv4": "203.0.113.10",
    "ssh_port": 22,
    "ssh_target": "[email hidden]",
    "ssh_health_command": [
      "/usr/local/sbin/anchor_guard.py",
      "--config",
      "/etc/anchor-guard.json",
      "local-health"
    ],
    "health_port": 9443,
    "health_path": "/healthz",
    "health_tls": true,
    "health_tls_insecure": false,
    "timeout_seconds": 4,
    "oob_evidence_file": "/var/lib/anchor-guard/oob-evidence.json",
    "oob_max_age_hours": 1
  },
  "hetzner": {
    "api_base": "https://api.hetzner.cloud/v1",
    "token_env": "HCLOUD_TOKEN_READ"
  },
  "billing": {
    "state_file": "/var/lib/anchor-guard/billing-state.json",
    "max_ack_age_hours": 168,
    "max_scan_age_hours": 30,
    "deadman_ping_url_env": "BILLING_DEADMAN_URL",
    "imap": {
      "host": "imap.example.net",
      "port": 993,
      "mailbox": "INBOX",
      "username_env": "BILLING_IMAP_USER",
      "password_env": "BILLING_IMAP_PASSWORD",
      "lookback_days": 45,
      "max_messages": 200,
      "allowed_from_domains": [
        "hetzner.com"
      ],
      "patterns": {}
    }
  },
  "local_health": {
    "state_dir": "/var/lib/Маяк",
    "systemd_unit": "Маяк.service",
    "heartbeat_file": "/run/Маяк/heartbeat.json",
    "heartbeat_max_age_seconds": 90,
    "min_free_bytes": 1073741824,
    "min_free_percent": 10,
    "min_inode_free_percent": 10,
    "max_clock_error_seconds": 2
  }
}
JSON

cat > /mnt/data/Маяк.service.example <<'UNIT'
[Unit]
Description=Fleet anchor duties
After=network-online.target time-sync.target
Wants=network-online.target time-sync.target

[Service]
Type=notify
ExecStart=/usr/local/bin/Маяк --config /etc/Маяк.json
Restart=on-failure
RestartSec=5s
WatchdogSec=60s
NotifyAccess=main
StartLimitIntervalSec=10min
StartLimitBurst=5
MemoryHigh=384M
MemoryMax=512M
OOMPolicy=kill
NoNewPrivileges=yes
PrivateTmp=yes
ProtectSystem=strict
ProtectHome=yes
ReadWritePaths=/var/lib/Маяк /run/Маяк

[Install]
WantedBy=multi-user.target
UNIT

cat > /mnt/data/anchor-guard.timer.example <<'UNIT'
[Unit]
Description=Read-only anchor triage probe

[Timer]
OnBootSec=2min
OnUnitActiveSec=5min
RandomizedDelaySec=20s
Persistent=true
Unit=anchor-guard.service

[Install]
WantedBy=timers.target
UNIT

cat > /mnt/data/anchor-guard.service.example <<'UNIT'
[Unit]
Description=Read-only anchor triage probe
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
EnvironmentFile=/etc/anchor-guard.env
ExecStart=/usr/local/sbin/anchor_guard.py --config /etc/anchor-guard.json triage
User=anchor-guard
Group=anchor-guard
NoNewPrivileges=yes
PrivateTmp=yes
ProtectSystem=strict
ProtectHome=yes
ReadOnlyPaths=/var/lib/anchor-guard
UNIT

chmod +x /mnt/data/anchor_guard.py
python3 -m py_compile /mnt/data/anchor_guard.py
python3 /mnt/data/anchor_guard.py --help | head -30

```

---

**ChatGPT:** · `gpt-5-6-pro`

Hetzner’s 10-day deadline and case-by-case enforcement make unknown billing a reboot veto. Diagnostics stay read-only; ledger-granted epochs fence failover, and ambiguous Telegram sends are never blindly retried.

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:docs.hetzner.cloud/reference/hetzner invoice API Hetzner Accounts billing"},{"q":"site:docs.hetzner.cloud/reference/hetzner payment invoices endpoint"},{"q":"site:docs.hetzner.cloud/reference/cloud invoice payment billing endpoint Hetzner API"},{"q":"site:github.com/hetznercloud hcloud invoice payment API"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
bash -lc ls -l /mnt/data/anchor_guard.py /mnt/data/anchor-guard.example.json /mnt/data/Маяк.service.example /mnt/data/anchor-guard.timer.example /mnt/data/anchor-guard.service.example && python3 -m py_compile /mnt/data/anchor_guard.py && sed -n '1,240p' /mnt/data/anchor_guard.py
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"open":[{"ref_id":"turn845278view0"},{"ref_id":"turn845278view1"},{"ref_id":"turn845278view3"},{"ref_id":"turn845278view5"},{"ref_id":"turn897847search1"},{"ref_id":"turn348993view0"},{"ref_id":"turn348993view1"},{"ref_id":"turn241176view2"},{"ref_id":"turn241176view5"},{"ref_id":"turn945062view5"},{"ref_id":"turn945062view6"},{"ref_id":"turn137326view1"},{"ref_id":"turn137326view3"},{"ref_id":"turn945062view7"},{"ref_id":"turn610475view0"},{"ref_id":"turn348993view4"},{"ref_id":"turn840554search0"},{"ref_id":"turn840554search2"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
bash -lc wc -l /mnt/data/anchor_guard.py
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"search_query":[{"q":"site:docs.kernel.org cgroup v2 memory.events oom_kill official"},{"q":"site:freedesktop.org systemd OOMPolicy memory.events Restart on-failure official"},{"q":"site:docs.kernel.org proc pressure memory io PSI official"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
bash -lc cat > /mnt/data/README-anchor-guard.md <<'MD'
# Anchor Guard — minimal deployment notes

`anchor_guard_core.py` is a 475-line, Python-stdlib-only reference implementation. It is intentionally **read-only** with respect to Hetzner: it diagnoses and emits a signed-by-hash evidence bundle, but never powers, reboots, rebuilds, or deletes a server.

## Placement

Install the same script in two places:

- **Always-on desktop hub / standby:** run `billing-scan` every 6 hours and `triage` every 5 minutes. This is the main diagnostic vantage and must not depend on the anchor's Telegram sender.
- **Hetzner anchor:** expose only `local-health` through a forced-command SSH key. The anchor should not be the only machine watching its own billing or liveness.

A third external dead-man endpoint is recommended only for "the billing scanner itself stopped running". It need not know secrets or fleet state.

## Required environment variables on the desktop hub

```text
HCLOUD_TOKEN_READ=<Hetzner project token with Read permission only>
BILLING_IMAP_USER=<mailbox username>
BILLING_IMAP_PASSWORD=<mailbox password/app-password>
BILLING_DEADMAN_URL=<optional external heartbeat URL>
```

Keep any Hetzner **Read & Write** token off the LLM-agent host, or expose it only through a tiny allow-listed action wrapper that re-runs triage and validates the current `diagnosis_hash`.

## First install

```bash
sudo install -m 0755 anchor_guard_core.py /usr/local/sbin/anchor_guard.py
sudo install -m 0640 anchor-guard-core.example.json /etc/anchor-guard.json
sudo install -d -m 0750 -o anchor-guard -g anchor-guard /var/lib/anchor-guard
```

Edit `/etc/anchor-guard.json` with the real server ID, DNS name, expected Primary IPv4, health endpoint, IMAP server, and file paths.

## Positive billing acknowledgment

The mail scanner deliberately cannot infer "paid" from silence. After checking **Hetzner Accounts → Billing** and seeing zero open invoices, record a positive acknowledgment:

```bash
sudo -u anchor-guard /usr/local/sbin/anchor_guard.py \
  --config /etc/anchor-guard.json billing-ack \
  --state clear --open-invoices 0 \
  --source 'Hetzner Accounts > Billing' \
  --operator Anton \
  --note 'Weekly positive check'
```

Any authenticated invoice, charge-failure, reminder, suspension-warning, suspension, or termination mail arriving after that acknowledgment invalidates it. Re-check Accounts and issue a new acknowledgment only after the account is actually clear.

Recommended policy:

- Billing mail scan: every 6 hours; dead-man alert if no successful scan for 30 hours.
- Positive Accounts acknowledgment: weekly and after every invoice/payment event.
- Immediate page on charge failure, reminder, suspension warning, suspension, or termination.
- Maintain 2–3 months of account credit as a passive buffer.

## Forced-command health key on the anchor

Create a dedicated unprivileged user that can read the service/cgroup/heartbeat state and write only the disposable health-test file in the fleet state directory. In its `authorized_keys`, force the health command:

```text
restrict,command="/usr/local/sbin/anchor_guard.py --config /etc/anchor-guard.json local-health" ssh-ed25519 AAAA... desktop-anchor-health
```

The script checks:

- free bytes, free percentage, and free inodes;
- a real create/write/fsync/unlink probe;
- systemd state and restart count;
- cgroup `memory.events`, including OOM kills;
- memory and I/O PSI files;
- semantic heartbeat age;
- chrony/timedatectl synchronization;
- boot ID.

## Peer-vantage probe file

A second machine or external probe should atomically publish a short timestamped file on the desktop hub:

```json
{
  "observed_at": "2026-07-16T23:30:00Z",
  "ssh_ok": false,
  "health_ok": false,
  "source": "secondary-vantage"
}
```

The triage tool will not call the node dead from one failed path alone.

## Out-of-band console evidence

A human or narrowly scoped console helper can record fresh evidence after inspecting Hetzner status and the VNC/rescue console:

```json
{
  "observed_at": "2026-07-16T23:31:00Z",
  "provider_status": "clear",
  "console_state": "guest_hung_reboot_safe",
  "operator": "Anton",
  "note": "Console frozen; no disk/fsck activity visible"
}
```

Accepted reboot-safe values in the reference script are:

- `guest_hung_reboot_safe`
- `kernel_panic_reboot_safe`

Without fresh billing-clear evidence, live provider state, two-vantage failure, and fresh out-of-band evidence, `reboot_candidate` remains false.

## Typical commands

```bash
# Off-anchor billing scanner
anchor_guard.py --config /etc/anchor-guard.json billing-scan

# Deterministic diagnosis
anchor_guard.py --config /etc/anchor-guard.json triage | tee /var/log/anchor-triage.json

# Guest-only check
anchor_guard.py --config /etc/anchor-guard.json local-health
```

`triage` emits:

- a single diagnosis code;
- ordered evidence with timestamps and source names;
- reboot vetoes;
- recommended next action;
- a canonical SHA-256 `diagnosis_hash`.

## Suggested write-action interlock

Do not add cloud writes to this script. A separate 30–50 line wrapper should accept only:

```text
power_on <diagnosis-file> <diagnosis-hash>
reboot   <diagnosis-file> <diagnosis-hash>
```

Before acting it must:

1. re-run `triage` itself;
2. require the supplied and newly calculated diagnosis hash to match;
3. require evidence age below five minutes;
4. require the exact allow-listed diagnosis (`POWERED_OFF` for power-on; `reboot_candidate=true` for reboot);
5. enforce one power action per incident and a cooldown;
6. append intent and provider Action result to the fleet ledger.

The LLM agent should never receive the write token or arbitrary shell access to this wrapper.

## Important limits

- Hetzner's documented Cloud API does not expose invoice/payment state. The positive account acknowledgment is therefore intentionally human- or bank-evidence-backed.
- Email delivery has no hard SLA and authentication headers may be stripped by some mailbox systems; such evidence becomes `UNKNOWN`, not `clear`.
- No probe can prove a cause when the provider control plane, network, and console are all unavailable.
- The script cannot make Telegram sends exactly once across a crash because Telegram has no documented idempotency key for `sendMessage`.
- A second server without epoch fencing makes split brain more likely, not less.
MD

cat > /mnt/data/fenced-role-protocol.md <<'MD'
# Fenced role protocol for arbiter, fold, and outbound-primary

This protocol assumes the existing append-only consensus ledger has one unambiguous notion of a **committed record**. A local shard append, heartbeat, DNS record, or priority rank is not a commit.

## Safety rules

1. Priority and heartbeat choose a candidate; they never grant authority.
2. Every role grant has a monotonically increasing `epoch`.
3. Every effect carries `(role, epoch, effect_id, grant_ref)`.
4. Internal consumers reject effects with an epoch lower than the highest committed epoch they have observed.
5. A holder stops producing effects before its lease can expire. If lease validity is uncertain, it quiesces.
6. A restarted process has no valid lease, regardless of what its local disk says.
7. If the fleet cannot establish a unique committed grant, the role is unavailable. It does not elect by optimism.

## Minimal ledger records

```json
{
  "type": "ROLE_GRANT",
  "role": "outbound-primary",
  "epoch": 42,
  "holder": "desktop-hub",
  "lease_ms": 120000,
  "grant_ref": "ledger-seq-or-hash",
  "predecessor_epoch": 41,
  "reason": "anchor-unreachable",
  "evidence_hash": "sha256-of-triage-bundle"
}
```

```json
{
  "type": "ROLE_RENEW",
  "role": "outbound-primary",
  "epoch": 42,
  "holder": "desktop-hub",
  "grant_ref": "original-grant-ref"
}
```

```json
{
  "type": "HANDOFF_PREPARE",
  "role": "outbound-primary",
  "from_holder": "desktop-hub",
  "from_epoch": 42,
  "to_holder": "anchor-hz1",
  "cutoff_seq": 9182
}
```

```json
{
  "type": "HANDOFF_DRAINED",
  "role": "outbound-primary",
  "epoch": 42,
  "cutoff_seq": 9182,
  "last_effect_seq": 9179,
  "ambiguous_effect_ids": ["msg-...-17"]
}
```

The next grant is always a new epoch. Epochs are never rolled back or reused.

## Lease timing

Illustrative starting values, to be fault-tested rather than treated as constants:

- lease: 120 seconds;
- renewal attempt: every 30 seconds;
- current holder quiesces after 60 seconds without a committed renewal;
- contender activates only after the previous lease plus a 15-second clock/network margin;
- no role operation while clock synchronization is unsafe or clock-error bound exceeds 2 seconds.

The holder measures its local stop deadline with a monotonic clock. UTC exists for audit and cross-node comparison, not for waiting. On restart, monotonic state is gone and the node must reacquire.

## Role-specific duplicate control

### Reversible arbiter

- `effect_id = proposal_id`.
- A decision is valid only when it references the active role epoch and the proposal is still undecided.
- Consumers apply only the first valid committed decision.
- Conflicting decisions or missing unique commit stop automation and escalate; they are never resolved from local timestamps.

### Fold node

Treat folding as a deterministic build:

```text
fold_id = SHA256(sorted shard-tip hashes || fold algorithm version || config hash)
```

- Write to a temporary path, fsync, then atomic rename.
- Emit a manifest with input tips, output hashes, algorithm version, role epoch, and grant reference.
- Recomputing the same `fold_id` is harmless.
- Consumers accept only a manifest committed under the active epoch.
- A hash mismatch or non-deterministic output is corruption, not a failover condition; stop and alert.

### Telegram outbound-primary

Use a durable outbox with these states:

```text
PENDING -> CLAIMED(epoch) -> ATTEMPTED -> ACKED(message_id)
                                  \
                                   -> UNKNOWN_SENT
```

- Stable `effect_id` is created when the outreach intent is committed.
- Before the network call, durably append `SEND_ATTEMPT(effect_id, epoch)`.
- On a successful Telegram response, append `SEND_ACK(effect_id, epoch, message_id)`.
- If the process dies after `SEND_ATTEMPT` but before a durable ACK, mark `UNKNOWN_SENT`; do not blindly retry.
- A human may inspect the Telegram chat and explicitly resolve `UNKNOWN_SENT` to `ACKED` or `RETRY_ALLOWED`.
- Include a small visible effect-id suffix in outreach where socially acceptable.

This chooses at-most-once behavior under ambiguity. Choosing automatic retry instead gives at-least-once behavior and permits duplicates. The sink cannot be made exactly-once without sink-side idempotency.

## Failover

1. Triage produces fresh evidence and a diagnosis hash.
2. The incumbent either releases cleanly or its lease expires.
3. The candidate syncs the ledger and all role state through the last committed sequence.
4. A new `ROLE_GRANT` with `epoch+1` is committed.
5. The candidate activates only after observing its own committed grant and the old lease safety margin.
6. Every effect check verifies the epoch immediately before the effect.

For the outbound role, the new holder additionally waits the full old lease/lock-delay because Telegram cannot validate an epoch token.

## Failback

Failback is a planned handoff, not reverse failover:

1. Recovered anchor enters `RECOVERING`; it does not preempt by priority.
2. It catches up and passes health checks continuously for a soak period (for example 10 minutes).
3. Incumbent commits `HANDOFF_PREPARE` with a cutoff sequence and stops claiming work beyond it.
4. Incumbent drains in-flight internal effects and records all ambiguous Telegram effects.
5. Incumbent commits `HANDOFF_DRAINED`.
6. Fleet commits a new `ROLE_GRANT` to the anchor with `epoch+1` and an activation barrier at/after the cutoff.
7. Anchor activates only after it has observed the drained record and caught up through the barrier.
8. Incumbent stops and remains standby.

If the incumbent dies during handoff, abandon the handoff record and perform ordinary lease-expiry failover with a new epoch. Never shortcut the lease because the preferred anchor is back.

## Unavoidable boundary

If the ledger cannot produce a unique committed grant during a network partition, and the external effect target cannot reject stale epochs, the fleet must choose between availability and split-brain safety. For money, irreversible changes, canon, and unsolicited outreach, choose safety and stop.
MD

cat > /mnt/data/anchor-billing-scan.service.example <<'UNIT'
[Unit]
Description=Off-anchor billing mailbox watchdog
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
EnvironmentFile=/etc/anchor-guard.env
ExecStart=/usr/local/sbin/anchor_guard.py --config /etc/anchor-guard.json billing-scan
User=anchor-guard
Group=anchor-guard
NoNewPrivileges=yes
PrivateTmp=yes
ProtectSystem=strict
ProtectHome=yes
ReadWritePaths=/var/lib/anchor-guard
UNIT

cat > /mnt/data/anchor-billing-scan.timer.example <<'UNIT'
[Unit]
Description=Scan billing mailbox four times daily

[Timer]
OnCalendar=*-*-* 00,06,12,18:15:00
RandomizedDelaySec=5min
Persistent=true
Unit=anchor-billing-scan.service

[Install]
WantedBy=timers.target
UNIT

# Update triage unit to use the compact script and permit reading evidence.
cat > /mnt/data/anchor-guard.service.example <<'UNIT'
[Unit]
Description=Read-only anchor triage probe
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
EnvironmentFile=/etc/anchor-guard.env
ExecStart=/usr/local/sbin/anchor_guard.py --config /etc/anchor-guard.json triage
User=anchor-guard
Group=anchor-guard
NoNewPrivileges=yes
PrivateTmp=yes
ProtectSystem=strict
ProtectHome=yes
ReadOnlyPaths=/var/lib/anchor-guard
UNIT

cat > /mnt/data/anchor-guard.timer.example <<'UNIT'
[Unit]
Description=Run deterministic anchor triage every five minutes

[Timer]
OnBootSec=2min
OnUnitActiveSec=5min
RandomizedDelaySec=20s
Persistent=true
Unit=anchor-guard.service

[Install]
WantedBy=timers.target
UNIT

# Package compact kit; keep the longer exploratory file out of the user-facing kit.
rm -f /mnt/data/anchor-guard-kit.zip
cd /mnt/data && zip -q anchor-guard-kit.zip \
  anchor_guard_core.py anchor-guard-core.example.json README-anchor-guard.md fenced-role-protocol.md \
  Маяк.service.example anchor-guard.service.example anchor-guard.timer.example \
  anchor-billing-scan.service.example anchor-billing-scan.timer.example
unzip -l /mnt/data/anchor-guard-kit.zip

```

---

**ChatGPT:** · `gpt-5-6-pro`

```
{"find":[{"ref_id":"turn861440view3","pattern":"spending alert"},{"ref_id":"turn861440view3","pattern":"alert"},{"ref_id":"turn861440view3","pattern":"limit"}],"response_length":"long"}
```

---

**ChatGPT:** · `gpt-5-6-pro`

```
bash -lc ls -l /mnt/data/anchor-guard-kit.zip /mnt/data/anchor_guard_core.py /mnt/data/README-anchor-guard.md /mnt/data/fenced-role-protocol.md && sha256sum /mnt/data/anchor-guard-kit.zip /mnt/data/anchor_guard_core.py && python3 -m py_compile /mnt/data/anchor_guard_core.py && wc -l /mnt/data/anchor_guard_core.py
```

---

**ChatGPT:** · `gpt-5-6-pro`

# DR26-07-16-HUB-05 — Operational hardening design

## Bottom line

Do not try to make the Hetzner VPS immortal. Make it **replaceable without duplicating authority or external effects**.

The incident exposed three separate control failures:

1. **Administrative power loss was invisible.** Billing is upstream of the VM power plane, but it was not monitored as infrastructure.
2. **Reboot was used as diagnosis.** A reboot should be the last bounded remediation after explicit preconditions, never the first probe.
3. **Anchor duties were attached to a machine rather than a fenced role grant.** Heartbeat and priority can nominate a candidate, but they cannot safely authorize effects.

The appropriate AK‑47 architecture is:

```text
Billing mailbox + positive account acknowledgment -----\
Hetzner read-only API ----------------------------------+--> deterministic guard
Two independent network probes ------------------------+       |
Guest semantic health + systemd/cgroup/clock ----------/       +--> diagnosis JSON
                                                               +--> SHA-256 diagnosis_hash
                                                               +--> reboot vetoes

LLM agent --------------------------------------------------> recommendation only
                                                               |
                                                     allow-listed action gateway
                                                     with separate write credential

Existing committed ledger --> ROLE_GRANT(epoch, lease) --> anchor or standby
                                      |
                           fencing for every role effect
```

The overriding policy should be:

> **Prefer a temporary loss of availability to an ambiguous or duplicate authority.**

That is especially important for Telegram outreach, canon, irreversible operations, and money.

---

# Ranked hardening plan

| Rank | Failure closed | Minimal AK‑47 mechanism | Change type | Confidence |
|---:|---|---|---|---|
| **1** | Rebooting through billing suspension or unknown account state | Make `billing != CLEAR_AND_FRESH` an unconditional power-action veto | Small code + config | **Established** |
| **2** | Unpaid invoice detected too late | Off-anchor billing-mail monitor, positive Accounts acknowledgment, external dead-man, 2–3 months account credit | Config + small code | **Established** |
| **3** | LLM guesses or acts on stale status | Read-only evidence collector producing deterministic JSON and `diagnosis_hash`; write token held only by a narrow action gateway | Small code | **Emerging** application of established controls |
| **4** | Wrong reboot for disk-full, OOM, daemon crash, or clock drift | Semantic `/healthz`, statvfs/fsync probe, systemd state, cgroup OOM counters, PSI, chrony status | Config + small code | **Established** |
| **5** | Anchor/desktop split brain | Committed per-role `ROLE_GRANT` with monotonically increasing epoch and bounded lease; heartbeat only selects candidate | Protocol + code | **Established** |
| **6** | Duplicate arbitration, folds, or Telegram messages | Stable `effect_id`; deterministic fold hashes; durable Telegram outbox with `UNKNOWN_SENT` | Code | **Established** |
| **7** | Failback race when preferred anchor returns | Sticky incumbent, recovery soak, drain cutoff, `HANDOFF_DRAINED`, then a new epoch | Protocol + code | **Established** |
| **8** | Desktop nominally standby but asleep/unready | No-sleep configuration, wired network, UPS, boot-enabled services, continuously synced state, recurring takeover drill | Config + hardware | **Established** |
| **9** | Maintenance, IP drift, or account loss destroys recovery path | Provider-status subscriptions, protected Primary IP, off-provider ledger/config backups and restore tests | Mostly config | **Established** |
| **10** | Hetzner account/region and home outage overlap | Small second-provider VPS, after fencing has been proven | Second node | **Established** architecture; ROI is contextual |
| **11** | Complexity itself becomes the new failure mode | Do not add Kubernetes, an additional Raft cluster, active-active Telegram, portal scraping, or autonomous payment | Policy | **Established** |

---

# 1. Billing and payment as an infrastructure failure mode

## What Hetzner actually exposes

As of **16 July 2026**, I found no documented public Hetzner API endpoint that returns invoice state, payment status, account arrears, or an impending billing suspension. The documented APIs cover infrastructure resources such as servers, IPs, volumes, firewalls, DNS and related products—not customer billing state. Therefore, an implementation that claims to determine “billing clear” from the Hetzner Cloud API is inventing a capability that is not publicly documented. citeturn861440view4

Hetzner’s documented sequence is approximately:

| Event | Earliest observable evidence | Timing |
|---|---|---|
| Invoice created | Billing email and Hetzner Accounts billing page | **T0** |
| Automatic payment attempted | Card/SEPA processing | A few days after invoice creation |
| Automatic payment fails | Failure email | At the failed attempt |
| Payment deadline/reminder | Billing email/account | Payment is expected within 10 days; reminder follows |
| Escalating warnings | Billing email | Hetzner describes four reminders; the third warns of account deactivation |
| Lock/suspension | Product becomes unavailable; provider/account evidence | Exact timing is not published and actions are case-specific |

Hetzner also says that enforcement can include locking individual services or the whole account, and that the timing depends on the case. Bank transfers may take a long time to be processed, making them unsuitable as a last-minute recovery mechanism. Hetzner Accounts exposes open invoices, and account credit can be maintained as a payment buffer. citeturn774493view0turn774493view3turn392955view0turn774493view4

Do not substitute Hetzner “cost alerts” for billing monitoring. Hetzner describes them as convenience notifications and does not guarantee timely delivery; they report estimated resource cost, not whether an invoice was successfully paid. citeturn912203view1

A small practical detail: Hetzner changed its invoice sender address in July 2026. Mail filtering should therefore validate authenticated `hetzner.com` mail and classify content, rather than pinning one exact sender address forever. citeturn774493view0

## Required billing state machine

Use explicit states rather than a Boolean:

```text
UNKNOWN
CLEAR
INVOICE_OPEN
PAYMENT_PENDING
PAYMENT_FAILED
OVERDUE
DEACTIVATION_WARNING
SUSPENDED
TERMINATION_WARNING
```

The safety rule is:

```text
Only CLEAR + fresh evidence permits a cloud power action.
Every other state vetoes reboot.
```

`CLEAR` must be based on **positive evidence**, for example:

- Hetzner Accounts shows zero open invoices.
- A provider payment receipt is matched to the invoice.
- A settled card/SEPA transaction is matched and the provider account no longer lists it as open.

Silence from the mailbox is not evidence of payment.

A useful evidence record is:

```json
{
  "provider": "hetzner",
  "state": "CLEAR",
  "observed_at": "2026-07-16T22:40:00Z",
  "valid_until": "2026-07-23T22:40:00Z",
  "invoice_ids_reconciled": ["R012345678"],
  "open_invoice_count": 0,
  "source": "Hetzner Accounts > Billing",
  "operator": "Anton",
  "raw_evidence_sha256": "..."
}
```

Any later invoice, charge failure, reminder, warning, or suspension mail invalidates that acknowledgment.

## Payment dead-man’s switch

There should be **two independent dead-men**:

### A. Scanner-liveness dead-man

The desktop or second provider node scans the billing mailbox every six hours. After a successful scan, it pings an external dead-man endpoint.

If no successful scan arrives for approximately 30 hours, alert through channels that do not depend on:

- the Hetzner anchor;
- the primary Telegram sender;
- ideally, the same email mailbox being monitored.

Dead-man services are intentionally simple: a periodic task reports success, and a missed report creates an alert. Multiple independent notification channels are preferable. citeturn861440view17

### B. Invoice-payment dead-man

When an invoice is seen, start a deadline that can only be cancelled by positive payment evidence.

A conservative internal policy—not a claimed Hetzner schedule—would be:

| Time from invoice | Fleet policy |
|---|---|
| T0 | Record invoice, notify informationally |
| T+3 days | Amber if no positive payment acknowledgment |
| T+5 days | Page Anton; verify payment method and open invoices |
| T+7 days | P0; sync standby, test secrets, prepare clean handoff |
| T+8 days | Voluntarily hand anchor duties to standby if still unresolved |
| Any payment-failure email | Immediate P0 and standby preparation |
| Any deactivation warning | Immediate planned handoff |
| Any suspension evidence | Provider/account incident; no reboot |

This intentionally moves duties **before** the provider’s 10-day payment boundary.

## Passive payment hardening

For a personal fleet, the highest-value measures are mundane:

- Maintain approximately **2–3 months of Hetzner account credit**.
- Use a dedicated payment method whose expiry is separately monitored.
- Put a recurring calendar check several weeks before card expiry.
- Preserve the customer number, invoice number, payment instructions and support route outside the anchor.
- Do not depend on emergency bank transfer near the deadline.
- If a second provider is introduced, avoid using the same expiring card as the only payment rail for both providers.

## Honest billing limit

You cannot make Hetzner billing perfectly observable with the documented Cloud API. Email can be delayed or filtered, the portal may require human authentication, and Hetzner does not publish a deterministic suspension timetable.

Therefore:

> Billing monitoring reduces probability and gives early warning. **Failover removes billing from the availability-critical path.**

Do not build a brittle browser scraper that logs into the billing portal and attempts autonomous payment. That creates a credential-handling and accidental-money-transfer system more dangerous than the outage it is meant to prevent.

---

# 2. Deterministic “anchor unreachable” triage

## The invariant

**Reboot is a remediation, not a diagnostic probe.**

Evidence collection may run in parallel, but the decision must follow this precedence:

```text
billing/account
    ↓
provider control plane and VM state
    ↓
identity, DNS and network path
    ↓
SSH and guest/service health
    ↓
out-of-band console/provider incident
    ↓
one bounded power action, if explicitly permitted
```

Google’s SRE guidance emphasizes simple, comprehensible critical monitoring, a distinction between symptoms and causes, and a combination of black-box and white-box evidence. Troubleshooting actions and their results should be recorded rather than repeatedly attempted from memory. citeturn861440view16turn604305view6turn604305view7

## Decision tree

### Stage 0 — Freeze mutation

On the first “anchor unreachable” signal:

1. Allocate an `incident_id`.
2. Stop automatic provider mutations.
3. Reject a second reboot/power request associated with the same incident.
4. Collect fresh evidence.

### Stage 1 — Account and billing

```text
billing state not CLEAR
or evidence expired
or billing scanner stale
    => NO REBOOT
       diagnosis = BILLING_RISK or BILLING_UNKNOWN
```

A billing warning is enough to veto. The system does not need to prove that billing caused the current outage.

### Stage 2 — Hetzner read-only API

Use a **Read-only** project token. Hetzner distinguishes read-only from read/write tokens, so there is no reason for the diagnostic process or LLM to hold a power-capable credential. citeturn604305view1

Evaluate:

| API evidence | Diagnosis | Action |
|---|---|---|
| 401/403 | Credential/account/control-plane unknown | No reboot; inspect account/token |
| Server missing | Wrong project, deleted resource or account problem | No reboot |
| `status=off` | VM powered off | One guarded **power-on**, not reboot |
| `starting`, `stopping`, `migrating`, `rebuilding`, etc. | Provider operation in progress | Wait and observe |
| `running` | Hypervisor believes guest is running | Continue to network tests |
| Server `locked=true` | Resource action/restriction exists | Inspect provider action/account evidence |
| Public IP `blocked=true` | Provider network restriction | No reboot; investigate restriction |

The Hetzner model exposes states such as running, off, starting, stopping, migrating, rebuilding and unknown, plus server-lock and network-block fields. These fields are useful evidence, but `locked` or `blocked` alone must not be labelled “billing”: Hetzner uses locks for multiple administrative and abuse-related causes and normally supplies explanatory account/email evidence. citeturn682600view0turn682600view1turn774493view8

Before any second cloud action, verify whether a previous provider action is still pending or failed. Never stack reboots.

### Stage 3 — Identity and network

Check:

1. API-reported IPv4/IPv6.
2. Expected protected Primary IP.
3. DNS A/AAAA records.
4. TCP connection to the health endpoint.
5. TCP connection to SSH.
6. Semantic HTTPS health response.
7. The same probes from at least one independent vantage.

Do not declare the host dead from ICMP alone. A firewall or path may drop ICMP while the service remains healthy.

Interpretation:

| Result | Likely class | Reboot? |
|---|---|---|
| DNS points to old IP | Discovery/config failure | No |
| One vantage fails, another succeeds | Path/ISP/firewall problem | No |
| SSH fails but semantic health succeeds | SSH-specific problem | No |
| API says running, both vantages fail | Continue to provider status/OOB | Not yet |
| Provider status reports incident | Provider failure | No; fenced failover |

Hetzner provides a public status facility and product-specific maintenance notifications for logged-in customers. Those subscriptions are cheap and should be enabled. citeturn774493view9turn102868view2

### Stage 4 — Guest and daemon health

When SSH or the semantic endpoint responds, collect:

```text
boot_id
daemon systemd ActiveState/SubState/Result
restart count
semantic heartbeat age
free bytes and free percentage
free inode percentage
real create/write/fsync/unlink test
cgroup memory.events
memory and I/O PSI
clock synchronized / offset / error bound
ledger tip and fold age
```

Systemd can restart a service on process failure, timeout, watchdog failure and OOM-related termination. `MemoryHigh` is appropriate for normal pressure control, while `MemoryMax` is the final hard stop. Cgroup `memory.events` gives explicit OOM/limit counters; PSI exposes sustained CPU, memory and I/O pressure; chrony tracking provides synchronization and dispersion information. citeturn774493view11turn774493view12turn311918search0turn311918search2turn774493view14

Response precedence:

- **Disk or inode exhaustion:** self-fence writers; rotate only explicitly disposable data; alert human. Reboot does not create space.
- **Filesystem write/fsync failure:** self-fence immediately; do not reboot blindly.
- **OOM-killed daemon:** let systemd restart the daemon once with backoff; alert on recurrence. Do not reboot the VM as the first response.
- **Daemon stopped while guest healthy:** restart the daemon once.
- **Clock unsafe:** self-fence arbiter, fold and outbound roles. Monitoring can remain active.
- **Ledger mismatch or corruption:** stop role effects; rebuild from verified shards or backup.
- **Clean guest but stale semantic heartbeat:** restart only the affected service, then fail over if it loops.

### Stage 5 — Out-of-band evidence

Before a reboot of a `running` but unreachable VM, check:

- Hetzner status/maintenance;
- provider console/VNC;
- rescue console where appropriate;
- whether the kernel is panicked or guest is genuinely frozen;
- whether visible disk recovery/fsck activity is in progress.

Hetzner documents the VNC console as an emergency access path. citeturn769212search4

### Stage 6 — Reboot eligibility

A reboot may be recommended only when **all** of these are true:

```text
billing == CLEAR and evidence is fresh
Hetzner API is reachable
server exists and status == running
server is not locked
public network is not blocked
expected IP identity matches
two independent vantages cannot reach SSH or semantic health
no provider incident or scheduled maintenance explains it
fresh OOB evidence says guest hung or kernel panicked
no disk-full/fsck/OOM diagnosis argues against reboot
no previous power action is pending
no power action has already been attempted for this incident
diagnosis age < 5 minutes
```

Then perform **one** reboot, observe the provider Action to completion, and rerun the full diagnosis. A second reboot must require a new incident decision, not “the first one did not help.”

---

# 3. Reference AK‑47 implementation

I built a stdlib-only reference kit implementing the read-only side of this design.

The core is **475 lines of Python** and provides:

```text
billing-scan    authenticated billing-mail classification
billing-ack     positive human/account acknowledgment
local-health    disk, fsync, systemd, cgroup, PSI, clock, heartbeat, boot ID
triage          ordered diagnosis with reboot vetoes and diagnosis_hash
```

Typical use:

```bash
anchor_guard.py --config /etc/anchor-guard.json billing-scan

anchor_guard.py --config /etc/anchor-guard.json triage \
  | tee /var/log/anchor-triage.json

anchor_guard.py --config /etc/anchor-guard.json local-health
```

The implementation is deliberately **read-only with respect to Hetzner**. It contains no power-on, reboot, rebuild or deletion endpoint.

The write side should be a separate, tiny allow-listed gateway:

```text
power_on <diagnosis-file> <diagnosis-hash>
reboot   <diagnosis-file> <diagnosis-hash>
```

Before acting, that gateway must:

1. Rerun triage itself.
2. Verify the supplied and freshly calculated diagnosis hashes.
3. Require all evidence to be within its TTL.
4. Require the exact allowed decision.
5. Enforce one power action per incident and a cooldown.
6. Record intent, provider response and Action result in the fleet ledger.
7. Redact the provider token from every diagnostic record.

The LLM receives neither the Hetzner write token nor arbitrary shell access to the gateway.

---

# 4. Anchor-duty failover without split brain

## Election is not authority

The existing rule—

> first machine in fixed priority order whose heartbeat is fresh

—is a useful **candidate-selection rule**. It is not a safe authority grant.

A heartbeat cannot prove that the previous holder has stopped. During a partition, both sides can have internally plausible but mutually stale views.

Use the existing committed ledger to create a per-role grant:

```json
{
  "type": "ROLE_GRANT",
  "role": "outbound-primary",
  "epoch": 42,
  "holder": "desktop-hub",
  "lease_ms": 120000,
  "grant_ref": "ledger-commit-seq-or-hash",
  "predecessor_epoch": 41,
  "reason": "anchor-unreachable",
  "evidence_hash": "sha256:..."
}
```

Every effect carries:

```text
(role, epoch, effect_id, grant_ref)
```

Recipients remember the highest committed epoch and reject lower epochs. This is the classic fencing-token pattern: a stale former holder may continue running, but its effects are no longer accepted. Google’s Chubby design uses monotonically increasing lock generations/sequencers for precisely this stale-holder problem and requires services receiving effects to reject stale generations. citeturn392955view7turn774493view16turn392955view4

## Minimal lease behaviour

Illustrative starting values:

```text
lease duration:                 120 seconds
renewal attempt:                 30 seconds
holder quiesces after:           60 seconds without committed renewal
new holder safety margin:        15 seconds after old lease boundary
maximum accepted clock error:     2 seconds
```

These are starting parameters to fault-test, not universal constants.

Rules:

- The holder may operate only after observing its own committed grant.
- Renewal extends authority only when the renewal is committed.
- If renewal validity becomes uncertain, the holder quiesces before nominal expiry.
- A process restart loses its lease, regardless of the contents of local disk.
- Unsafe clock state self-fences all effect-producing roles.
- Epoch numbers are never reused or rolled back.
- A new holder activates only after the old lease/lock-delay and its own committed `epoch+1` grant.

Conservative quiescing during lease uncertainty is standard lease practice; a new primary epoch prevents calls from an old primary from being accepted. citeturn392955view5turn392955view6

## Human super-crown

Anton’s live-presence authority should remain a **separate short lease**, not a Boolean status file:

```json
{
  "type": "HUMAN_PRESENCE_LEASE",
  "session_id": "anton-terminal-...",
  "epoch": 301,
  "last_input_seq": 1782,
  "expires_after_ms": 30000
}
```

It should be renewable only by the trusted interactive session and expire within tens of seconds without input.

A canon, irreversible or money effect requires both:

```text
valid role grant
AND
valid current human-presence lease
```

Failover never transfers or fabricates the super-crown.

---

## Role-specific duplicate avoidance

### A. Reversible proposal arbiter

Use:

```text
effect_id = proposal_id
```

A decision is valid only when:

- it references the active arbiter epoch;
- the proposal is still undecided;
- no valid committed decision for that `proposal_id` already exists.

The first valid committed decision wins. Conflicting local decisions are not reconciled by timestamps; automation stops.

### B. Fold node

Treat folding as a deterministic build:

```text
fold_id =
    SHA256(
        sorted input shard-tip hashes
        || fold algorithm version
        || canonicalization version
        || config hash
    )
```

Write output as:

1. Temporary file.
2. Flush.
3. `fsync` file.
4. Atomic rename.
5. `fsync` containing directory.
6. Commit manifest.

Manifest:

```json
{
  "fold_id": "...",
  "input_tips": ["..."],
  "algorithm_version": 7,
  "config_hash": "...",
  "output_hashes": {"canon.json": "..."},
  "role_epoch": 42,
  "grant_ref": "..."
}
```

Duplicate computation is harmless. Consumers accept only manifests committed under the active epoch.

A hash mismatch is corruption, not a reason to elect another fold node and continue optimistically.

### C. Telegram outbound-primary

Use a durable outbox:

```text
PENDING
  -> CLAIMED(epoch)
  -> ATTEMPTED
  -> ACKED(telegram_message_id)

ATTEMPTED
  -> UNKNOWN_SENT
```

Protocol:

1. Commit outreach intent and stable `effect_id`.
2. Active holder claims it under current epoch.
3. Before the API call, durably commit `SEND_ATTEMPT`.
4. Call Telegram.
5. On a successful response, commit `SEND_ACK` with returned message ID.
6. If the process dies between steps 4 and 5, mark it `UNKNOWN_SENT`.
7. Do not automatically retry an `UNKNOWN_SENT` message.

Telegram’s documented `sendMessage` operation returns a Message on success but does not document an idempotency-key parameter. Therefore a crash after Telegram accepts the request but before the local ACK is durably stored cannot be made exactly once by the sender alone. citeturn774493view15

You must choose:

- **At-most-once:** ambiguous sends are not automatically retried. Possible omission.
- **At-least-once:** ambiguous sends are retried. Possible duplicate.

For unsolicited outreach, at-most-once is generally the less damaging choice. A short visible effect-ID suffix can aid manual reconciliation.

Because Telegram cannot validate your epoch fencing token, the new sender must also wait the full previous lease/lock-delay before sending.

---

# 5. Clean failover and failback

## Failover

1. Deterministic triage emits a fresh evidence bundle and hash.
2. The incumbent cleanly releases, or its lease expires.
3. Candidate synchronizes the committed ledger and relevant role state.
4. Candidate resolves all known `UNKNOWN_SENT` and in-flight records conservatively.
5. Commit `ROLE_GRANT(epoch+1, candidate)`.
6. Candidate observes its own committed grant.
7. Candidate waits the old-holder safety boundary.
8. Candidate starts effects.
9. Every effect rechecks epoch immediately before committing or calling an external sink.

If the existing ledger cannot produce a unique committed role grant while the anchor is down, automated takeover is not safe. The correct result is role unavailability until the ledger or Anton can establish unique authority.

## Failback

Failback must not happen merely because the preferred anchor’s heartbeat reappears.

Use a planned handoff:

1. Recovered anchor enters `RECOVERING`.
2. It has **zero role authority** despite its higher priority.
3. It catches up ledger, shards, fold manifests and outbox.
4. It passes health, disk, memory and clock checks continuously for a soak period, for example ten minutes.
5. Incumbent commits:

```json
{
  "type": "HANDOFF_PREPARE",
  "role": "outbound-primary",
  "from_epoch": 42,
  "to_holder": "anchor-hz1",
  "cutoff_seq": 9182
}
```

6. Incumbent stops claiming work beyond `cutoff_seq`.
7. Incumbent drains internal effects and records ambiguous Telegram sends.
8. Incumbent commits:

```json
{
  "type": "HANDOFF_DRAINED",
  "role": "outbound-primary",
  "epoch": 42,
  "cutoff_seq": 9182,
  "ambiguous_effect_ids": ["outreach-9177"]
}
```

9. Commit a new anchor grant at `epoch=43`.
10. Anchor activates only after observing the drain record, catching up through the cutoff and passing the activation boundary.
11. Desktop becomes standby.

If the desktop dies during handoff, abandon that handoff and use ordinary lease-expiry takeover with another new epoch. Never reuse epoch 42 or shortcut the delay because the preferred machine has returned.

For a personal fleet, **manual or semi-automatic failback is preferable**. Failover protects availability; aggressive automatic failback mostly creates races.

---

# 6. Desktop standby versus a second-provider anchor

## Coverage comparison

| Option | Protects against | Does not protect against | Recommendation |
|---|---|---|---|
| Second VPS in same Hetzner account | Individual host or VM failure | Hetzner account suspension, billing failure, broad region/control-plane failure | Low value for this incident class |
| Always-on desktop hub | Hetzner billing/account/region failure | Home power, ISP, sleep, travel shutdown, domestic hardware failure | First standby to implement |
| Small VPS at another provider | Hetzner billing/account/region and home outage | Shared payment method, shared software bugs, shared secret compromise | Add only after fencing works |

A different-provider entry VM is not inherently expensive: current public entry pricing is approximately **$4/month at DigitalOcean** and **$4.54/month at OVHcloud**, before optional backups, taxes, IP or traffic differences. Exact regional pricing should be checked at purchase time. citeturn604305view4turn604305view5

The second node does not need to run an LLM. It only needs capacity for:

- ledger synchronization;
- deterministic folds;
- the outbound queue;
- health/billing observers;
- the role agent;
- retained logs.

## When the second cloud node is worth it

It becomes rational when several of these are true:

- Unattended recovery objective is below roughly 10–15 minutes.
- Anton frequently travels.
- The desktop cannot be guaranteed wired, powered, no-sleep and boot-recoverable.
- Telegram continuity has real operational or reputational cost.
- A home ISP/power outage overlapping a provider incident is unacceptable.
- Failover and failback drills already pass reliably.
- The additional provider account and payment rail will actually be monitored.

It is probably not worth it when:

- Several hours of outage are acceptable.
- Canon, money and irreversible effects already stop without Anton.
- Telegram outreach is delay-tolerant.
- The desktop is genuinely always-on and UPS-backed.
- The second node would rarely be patched or tested.

A second server introduced before fencing is not redundancy; it is an additional split-brain participant.

## Identity and DNS

Use three separate concepts:

```text
node_id        immutable machine identity
role           arbiter / fold / outbound-primary
discovery name current network location
```

Rules:

- Unique SSH host keys and `machine-id` per node.
- Unique cloud API credentials per node.
- Never clone a full VM image with live node identity or lease state.
- DNS is discovery, not leadership.
- Peers should know at least two endpoint addresses and verify node identity cryptographically.
- Telegram outbound does not require inbound DNS at all.
- A protected Hetzner Primary IP can simplify rebuilding inside Hetzner, but it is not portable to another provider. Hetzner supports retaining/reassigning Primary IPs when configured not to be auto-deleted. citeturn774493view10

## Secret distribution

For this fleet size:

- Use root-readable files or operating-system credential facilities.
- Use per-node credentials wherever possible.
- Put only the read-only Hetzner token on the diagnostic node.
- Keep the write token in the narrow action gateway or offline.
- Never share an SSH private key between nodes.
- The Telegram token must be available to both potential senders, but only the active fenced role may use it.
- Rotate the Telegram token after suspected node compromise.
- Do not invent stdlib-only encryption. Use an established secret-encryption mechanism or manual provisioning.

The second provider should ideally use a different payment rail; otherwise an expired or blocked card remains a correlated failure.

---

# 7. Hosted-anchor failure catalogue

The auto-heal rule should be:

> Automatically heal only a bounded, idempotent, well-diagnosed failure. Otherwise self-fence and alert.

| Failure mode | Cheapest reliable signal | Correct automatic response | Human response |
|---|---|---|---|
| Invoice issued but not acknowledged | Authenticated billing email + invoice state | Start payment deadline | Verify open invoices/payment |
| Automatic payment failed | Authenticated provider failure email | P0 alert; prepare standby | Fix payment immediately |
| Overdue/deactivation warning | Reminder/warning classification | Planned role handoff | Pay/contact provider |
| Account or product suspended | Provider mail/account evidence; blocked/locked resource fields | Self-fence and fail over | Resolve account/provider issue |
| VM genuinely powered off | Read-only Cloud API reports `off` | One guarded power-on | Investigate if repeated |
| Provider action in progress | API transitional/locked state | Wait; suppress duplicate action | Inspect failed/stuck action |
| Host or region incident | Provider status + two external probes | Fenced failover after lease | Follow provider incident |
| Scheduled maintenance | Status subscription/account notice | Pre-stage or voluntarily hand off | None unless service fails |
| One network path broken | Two-vantage TCP/HTTPS probes | Continue on reachable path | Inspect route/firewall |
| DNS or IP drift | API IP versus expected IP and DNS | Normally alert; no reboot | Correct DNS/config |
| SSH daemon failure | Semantic health works but SSH fails | Restart SSH only where safely allowed | Inspect config/auth |
| Anchor daemon crash | systemd state/watchdog | `Restart=on-failure` once/backoff | Inspect repeated crash |
| Daemon deadlock | Semantic heartbeat stale, process still present | Watchdog restart; then fail over | Inspect stack/logs |
| OOM kill | `memory.events`, systemd result, PSI | Restart service once with rate limit | Correct limits/leak/capacity |
| Disk full | Free bytes/inodes and failed fsync canary | Self-fence writers; bounded log rotation only | Decide what can be deleted |
| Read-only/corrupt filesystem | Write/fsync failure, kernel logs/OOB | Self-fence | Repair/restore |
| Clock drift | chrony sync/error threshold | Self-fence all authority roles | Fix NTP/network |
| Ledger lag | Commit-tip age and peer comparison | Stop effects until caught up | Inspect replication |
| Ledger/hash corruption | Hash-chain or fold-manifest mismatch | Stop role; rebuild verified state | Investigate corruption |
| Unexpected reboot | `/proc/.../boot_id` changed | Require a new lease; old lease invalid | Investigate cause |
| API or bot token revoked | Live authenticated no-op/read probe | Stop affected function | Rotate credential |
| Telegram ambiguous send | Durable `ATTEMPTED` without ACK | Mark `UNKNOWN_SENT`; no retry | Reconcile and explicitly resolve |
| Backup silently stale | Restore-test heartbeat and verified tip | Alert | Repair backup path |

---

# 8. Evidence-forced LLM operation

Treat the LLM as an **untrusted planner and summarizer**, not as a sensor or power controller.

## Evidence schema

Every observation should contain:

```json
{
  "evidence_id": "hcloud-server-state",
  "source": "Hetzner Cloud API GET /servers/123",
  "observed_at": "2026-07-16T23:01:03Z",
  "received_at": "2026-07-16T23:01:03.221Z",
  "ttl_seconds": 300,
  "boot_id": null,
  "parsed": {
    "status": "running",
    "locked": false,
    "ipv4_blocked": false
  },
  "raw_sha256": "..."
}
```

For guest evidence, include the guest `boot_id`. A status file from a previous boot is invalid even if its embedded timestamp looks recent.

Use both:

- remote-reported observation time;
- local receipt time.

A remote machine with a bad clock must not extend its own evidence validity.

## Deterministic diagnosis

The policy engine, not the LLM, emits:

```json
{
  "incident_id": "inc-20260716-0017",
  "decision": "REBOOT_VETO_BILLING_UNKNOWN",
  "reboot_candidate": false,
  "recommended_action": "CHECK_HETZNER_ACCOUNTS_BILLING",
  "required_evidence_missing": ["billing-positive-ack"],
  "diagnosis_hash": "sha256:..."
}
```

The LLM may explain this result, but cannot replace it with its own preferred diagnosis.

## Proof-carrying action request

An LLM recommendation should be valid only in a structured form such as:

```json
{
  "requested_action": "reboot_once",
  "incident_id": "inc-20260716-0017",
  "diagnosis_hash": "sha256:...",
  "evidence_ids": [
    "billing-clear",
    "hcloud-server-state",
    "probe-desktop",
    "probe-external",
    "oob-console"
  ]
}
```

The gateway ignores the prose and verifies the predicates itself.

Mandatory rules:

- Missing evidence becomes `UNKNOWN`, not an inferred success.
- Expired evidence is unusable.
- “Node unreachable” is a symptom, not a cause.
- The LLM must not use a cached status file unless the guard says it is fresh.
- A claimed cause must cite the exact live evidence IDs supporting it.
- The LLM cannot run arbitrary `curl` against provider write endpoints.
- The LLM cannot access payment credentials.
- Every action and result goes into the append-only incident record.
- The gateway observes the provider Action result before allowing another action.

This converts the LLM from “agent with root and opinions” into “operator interface over a deterministic state machine.”

---

# 9. Failure-injection acceptance tests

Do not call the design complete until these tests pass:

| Injection | Required outcome |
|---|---|
| Delete/expire billing acknowledgment | Reboot becomes impossible |
| Insert authenticated payment-failure fixture | P0 alert and standby preparation |
| Stop billing scanner | External dead-man alerts |
| Cloud API says `off` | Suggest power-on, never reboot |
| Cloud API token returns 403 | Control-plane unknown; no power action |
| One probe path blocked | No dead-node declaration |
| Both paths blocked but health works locally | Network diagnosis, no reboot |
| Fill disk/inodes | Writer self-fences; no VM reboot |
| OOM-kill anchor daemon | One service restart; repeated failure triggers failover |
| Offset clock beyond threshold | Role self-fences |
| Partition anchor from ledger | Anchor stops before lease expiry |
| Restore anchor while desktop active | Anchor remains `RECOVERING`; no priority preemption |
| Crash sender after Telegram API call, before ACK | Message becomes `UNKNOWN_SENT`; no automatic resend |
| Run fold concurrently on old/new node | Only current-epoch manifest is accepted |
| Kill incumbent during failback | New ordinary epoch takeover; no epoch reuse |
| Present stale status from previous boot | Rejected by `boot_id`/TTL |

Run at least the billing, disk, OOM, partition, ambiguous-send and failback tests after every material protocol change.

---

# 10. What a personal fleet should deliberately not solve

Accept and monitor these boundaries:

1. **No guaranteed pre-suspension signal from Hetzner Cloud API.** Use mail/account evidence, credit buffer and failover.
2. **No exact-once Telegram sending across an ambiguous crash.** Choose omission or duplicate risk explicitly.
3. **No simultaneous availability and split-brain safety when the ledger cannot establish unique authority.** Fail closed.
4. **No protection from account deletion without an off-provider backup and tested restoration.**
5. **No trustworthy autonomous diagnosis when provider API, network and OOB console are all unavailable.** The cause is `UNKNOWN`.
6. **No safe automatic cleanup of an unknown full disk.** Only known disposable paths may be pruned automatically.
7. **No safe autonomous payment system operated by an LLM.**
8. **No benefit from building a second consensus stack beside the existing ledger.**
9. **No need for Kubernetes, service meshes, BGP, anycast, active-active Telegram or a multi-cloud secret broker.**
10. **No reason to optimize failback speed.** Slow, sticky failback is safer.

The personal-fleet target is not five-nines. It is:

- early billing warning;
- bounded role outage;
- no duplicate authority;
- no repeated destructive remediation;
- simple human recovery;
- evidence that survives the failed node.

---

# Deliverables

- [Download the complete anchor-guard kit](sandbox:/mnt/data/anchor-guard-kit.zip)
- [Inspect the 475-line stdlib core](sandbox:/mnt/data/anchor_guard_core.py)
- [Read the deployment README](sandbox:/mnt/data/README-anchor-guard.md)
- [Read the fenced role and failback protocol](sandbox:/mnt/data/fenced-role-protocol.md)

The ZIP includes the Python core, example JSON configuration, systemd service/timer examples, billing scanner timer and the role protocol.

```text
anchor-guard-kit.zip
SHA-256 6f7c6c8d860154223bf0fa2c86f1571d68a031c3876bc1dd235e254da44f3e0f

anchor_guard_core.py
SHA-256 2afdffd6c5af807c8563a68c0318d1726af1c37859f8d2fbdf217270d64e8b2d
```

The central invariant is simple: **a machine may disappear; authority must never become ambiguous.**
