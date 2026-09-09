# Memory Functionality Audit — Full-System Procedure

Runnable, self-contained procedure for the OpenCode agent. Invoked whenever
the developer asks for a "memory functionality audit", a "memory audit", or
a "state of the memory system report" — or after any restart, repair, or
suspicion. The agent reads this document and executes it start to finish,
in order, without skipping checks.

**Report convention (mandatory):** the audit report is saved to
`C:\mybizz\memory-audit-reports\memory-audit-(date).md` (date = the audit
date, `YYYY-MM-DD`). If remediation follows, the remediation report is
**appended to the same audit document** under a `## Remediation` heading
with its own date — never a separate file. This folder is exempt from the
cross-folder rule (always-open); audits work from any project.

---

## Section 0 — Principles

1. **Never invent a result.** Every check runs for real; the report
   carries the real output or a faithful summary of it.
2. **Read-only first.** Checks 1–7 are read-only. Check 8 (the end-to-end
   write) submits and then fully cleans up one disposable test candidate —
   run it only when the earlier checks give no reason to stop, and always
   complete its cleanup.
3. **Never read, print, or copy credential values.** The `.env` file is
   sourced into the environment for credentialed checks; values are
   referenced by variable name only, never displayed.
4. **A check that cannot run is a finding**, not a skip — report exactly
   what blocked it.
5. **The steady state is "No defects"**: every check passes, both
   processes running, no backlog, no review-blocking items unreported,
   no uncommitted register writes. Anything else is a defect list.

---

## Section 1 — Environment and process checks

### Check 1 — GBrain core
```bash
gbrain --version 2>&1 | head -1
gbrain sources status 2>&1 | head -15
pg_isready -h localhost -p 5432
```
Pass: version prints; all sources 100% embedded, no permanent failures;
PostgreSQL accepting connections. Fail: report the real output.

### Check 2 — API process
```bash
ss -tlnp | grep 8321
curl -s -o /dev/null -w "HTTP %{http_code}\n" --max-time 8 http://127.0.0.1:8321/memory/review-queue
```
Pass: something LISTENs on 8321; the curl returns **HTTP 422**
(credential-required — proves the API is up AND its auth gate works).
HTTP 000/connection refused = **API DOWN — DEFECT**. Remediation: the
documented start procedure (reference §Starting).

### Check 3 — Worker process
```bash
ps aux | grep "app.worker" | grep -v grep
tail -20 /tmp/memory-governor-worker.log 2>/dev/null
```
Pass: a live `app.worker` process; log shows no `permanently_failed`
entries. No process = **WORKER DOWN — DEFECT** (promoted candidates then
silently strand in the outbox). Remediation: start command in
`memory-governor-reference.md` §Starting.

### Check 4 — Outbox backlog (unprocessed and failed events)
```bash
cd /home/dev-p/memory-governor && set -a && source .env && set +a
.venv/bin/python -c "
import psycopg, os
with psycopg.connect(os.environ['MEMORY_GOVERNOR_DATABASE_URL']) as conn:
    with conn.cursor() as cur:
        cur.execute('SELECT count(*) FROM memory_events WHERE processed_at IS NULL AND permanently_failed = FALSE')
        print('unprocessed (pending) events:', cur.fetchone()[0])
        cur.execute('SELECT count(*) FROM memory_events WHERE permanently_failed = TRUE')
        print('permanently_failed events:', cur.fetchone()[0])
"
```
Pass: pending = 0 (or a small number while the worker is actively
draining — re-check after 60s: it must decrease). permanently_failed > 0
= **DEFECT** — every one is a real, surfaced delivery failure; report
each event's `event_type`/`last_error`.

### Check 5 — Review queue state
```bash
curl -s -H "X-Governor-Credential: $MEMORY_GOVERNOR_WRITER_MANUAL_SUBMISSION" http://127.0.0.1:8321/memory/review-queue
```
Pass: returns a list (possibly empty) — this is a **state report**, not a
defect: every open item is listed in the audit report with its memory_id
and classification, because open reviews block PDLF steps.

### Check 6 — Register integrity
```bash
cd /home/dev-p/memory-governor && git status --porcelain registers/ | head -5
.venv/bin/python -c "
import json, pathlib, re
index = pathlib.Path('registers/register-memory-governor.md').read_text()
ids = re.findall(r'\| ([0-9a-f-]{36}) \|', index)
missing = [i for i in ids if not pathlib.Path(f'registers/candidates/{i}.json').exists()]
print(f'index rows: {len(ids)}; payload files missing: {len(missing)}')
if missing: print('MISSING:', missing)
"
```
Pass: register folder clean in git (uncommitted = **DEFECT** — the worker's
push failed); every index row has its payload file; no orphaned payloads.

### Check 7 — Restart-stability posture
```bash
systemctl --user status memory-governor-api memory-governor-worker --no-pager 2>&1 | head -6
grep -c "systemd=true" /etc/wsl.conf 2>/dev/null
```
State report: if the units exist and are active — the permanent stage is
installed (ideal). If not — the system is start-on-demand and the
permanent auto-start migration is still pending developer execution;
report as scheduled work, not a defect.

---

## Section 2 — The end-to-end governed write (Check 8, heavyweight)

Runs the real pipeline once: submit → decide → worker → GBrain → register
→ verify → clean up. Reuses the proven Step 12.2 pattern. Run only when
Checks 1–4 pass. Cleanup is mandatory and unconditional:

```bash
cd /home/dev-p/memory-governor && set -a && source .env && set +a
.venv/bin/python - <<'EOF'
import json, os, pathlib, re, subprocess, time
from datetime import datetime, timezone

marker = f"AUDIT-MARKER-{int(time.time())}"
app = pathlib.Path('.').resolve()

# 1. Submit through the real API (manual credential from the environment)
from fastapi import FastAPI
from fastapi.testclient import TestClient
from app.api import router
from app.storage import get_connection, insert_project
from app.worker import run_polling_pass

test_app = FastAPI(); test_app.include_router(router)
client = TestClient(test_app)
project_id = "test-audit-write"
headers = {"X-Governor-Credential": os.environ["MEMORY_GOVERNOR_WRITER_MANUAL_SUBMISSION"]}

# --- Disposable GBrain source (REQUIRED, added 2026-09-09): the delivery
# coupling demands a real gbrain source matching owning_project — without
# it the write fails with SourceNotFoundError, exactly as documented. ---
tmp = tempfile.mkdtemp()
src_path = os.path.join(tmp, "gbrain_source")
os.makedirs(src_path)
subprocess.run(["git", "init", src_path], check=True, capture_output=True)
subprocess.run(["git", "-C", src_path, "config", "user.email", "audit@local"], check=True, capture_output=True)
subprocess.run(["git", "-C", src_path, "config", "user.name", "Audit"], check=True, capture_output=True)
open(os.path.join(src_path, "README.md"), "w").write("disposable audit source\n")
subprocess.run(["git", "-C", src_path, "add", "-A"], check=True, capture_output=True)
subprocess.run(["git", "-C", src_path, "commit", "-m", "initial"], check=True, capture_output=True)
subprocess.run(["gbrain", "sources", "add", project_id, "--path", src_path], check=True, capture_output=True)
current = subprocess.run(["gbrain", "sources", "current"], check=True, capture_output=True, text=True)
original_source = current.stdout.strip().splitlines()[0].replace("source: ", "").strip()

def cleanup_db(memory_id=None):
    with get_connection() as conn:
        with conn.cursor() as cur:
            if memory_id:
                cur.execute("DELETE FROM audit_log WHERE memory_id = %s", (memory_id,))
                cur.execute("DELETE FROM memory_events WHERE memory_id = %s", (memory_id,))
            cur.execute("DELETE FROM memory_candidates WHERE owning_project = %s", (project_id,))
            cur.execute("DELETE FROM projects WHERE project_id = %s", (project_id,))
        conn.commit()

def expire_gbrain_fact():
    r = subprocess.run(["gbrain", "call", "recall", json.dumps({"grep": marker})], capture_output=True, text=True)
    try:
        for f in json.loads(r.stdout).get("facts", []):
            fid = f.get("fact_id") or f.get("id")
            if fid:
                subprocess.run(["gbrain", "call", "forget", json.dumps({
                    "id": fid,
                    "reason": "memory-functionality-audit cleanup: disposable audit-marker test fact expired by its own audit run",
                })], check=True, capture_output=True)
    except (json.JSONDecodeError, subprocess.CalledProcessError):
        print("WARNING: gbrain cleanup incomplete; leftover audit-marker fact")

cleanup_db(); memory_id = None
try:
    insert_project(project_id, "Memory Functionality Audit")
    resp = client.post("/memory/candidate", headers=headers, json={
        "owning_project": project_id, "visibility": "project_local", "retention": "ephemeral",
        "memory_type": "fact", "content": {"text": f"{marker}: audit end-to-end write."},
        "source_type": "user", "source_ref": "/tmp/memory-audit-e2e.md", "policy_version": "v1",
        "confidence": {"score": 0.95, "method": "human_verified"},
    })
    print("submit:", resp.status_code, resp.json().get("status"))
    memory_id = resp.json()["memory_id"]
    print("worker pass processed:", run_polling_pass())
    subprocess.run(["gbrain", "sources", "default", project_id], check=True, capture_output=True)
    recall = subprocess.run(["gbrain", "call", "recall", json.dumps({"grep": marker, "entity": project_id})], capture_output=True, text=True, check=True)
    ok_presence = marker in recall.stdout
    ok_scoped = '"visibility": "private"' in recall.stdout
    register_row = marker in pathlib.Path("registers/register-memory-governor.md").read_text() or memory_id in pathlib.Path("registers/register-memory-governor.md").read_text()
    print("gbrain presence:", ok_presence, "| private visibility:", ok_scoped, "| register written:", register_row)
finally:
    cleanup_db(memory_id); expire_gbrain_fact()
    subprocess.run(["gbrain", "sources", "default", original_source], check=False, capture_output=True)
    # remove the disposable project's gbrain source registration if one was created
    subprocess.run(["gbrain", "sources", "remove", project_id, "--confirm-destructive"], check=False, capture_output=True)
print("END-TO-END: done (cleanup complete)")
EOF
```
Pass: submit 201 `promoted`; worker processed ≥ 1; GBrain presence true;
**private visibility true** (the 2026-09-08 mapping); register written
true. Any false = **DEFECT** with the exact assertion that failed.

---

## Section 3 — Report format (mandatory)

Write the report to `C:\mybizz\memory-audit-reports\memory-audit-(date).md`:

```markdown
# Memory Functionality Audit — (date)

Executor: OpenCode agent (per memory-functionality-audit.md)

## Results

| # | Check | Result | Evidence |
|---|---|---|---|
| 1 | GBrain core | pass/fail | ... |
| 2 | API process | pass/fail | ... |
| 3 | Worker process | pass/fail | ... |
| 4 | Outbox backlog | pass/fail (pending=N, failed=N) | ... |
| 5 | Review queue | state: N open items | memory_ids + classifications |
| 6 | Register integrity | pass/fail | ... |
| 7 | Restart-stability posture | installed / pending | ... |
| 8 | End-to-end governed write | pass/fail | ... |

## Defects
(none — steady state "No defects" confirmed) / the defect list

## Open review items
(state from Check 5 — every item with its memory_id and classification)

## Steady-state verdict
(one paragraph: is the memory system fully functional and in a steady
state; what, if anything, is scheduled rather than broken)
```

## Section 4 — Remediation

Remediate what is remediable in-session (restart a process, drain a
backlog, re-verify), then **append to the same audit document**:

```markdown
## Remediation (appended (date))
| Defect | Action taken | Re-check result |
|---|---|---|
```
Report anything not remediable in-session separately (commands prepared
for the developer), per the standing requirement: lack of permissions is
never a reason for failure — request the specific `mg-grant --path
<folder> --count N` needed, or hand the exact command to the developer for
protected paths.

---

## Section 5 — Known structural boundaries (report, never "fix")

- **GBrain's dream cycle is structurally ungovernable** through the
  governor's API — governed by the documented manual discipline (run
  `dream`, then `gbrain eval suspected-contradictions`, review). A
  permanent property of the third-party tool.
- **GBrain's facts layer is brain-global** — governed-memory isolation is
  enforced by the governor (entity stamping + visibility mapping), within
  the platform's real capabilities.
- **Phase 13.4/16.2/16.3 are deliberately parked** pending PDLF's go-live
  — scheduled external dependency, not a defect.

*The steady state this procedure exists to confirm: "No defects" — the
memory system fully functional, both processes running, every delivery
path proven, every known boundary honestly named.*
