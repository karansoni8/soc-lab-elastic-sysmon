# Troubleshooting Log

This lab included real setup issues that required log-based troubleshooting and validation.

## Kibana not reachable / crashing
**Symptom**
- Kibana UI not opening from Windows (`http://10.10.10.2:5601`)
- Kibana container restarting / failing

**Evidence**
- Checked container logs:
  - `docker logs kibana --tail=120`

**Fix**
- Corrected Kibana configuration/auth approach (avoided forbidden superuser usage)
- Recreated Kibana container:
  - `docker compose up -d --force-recreate kibana`

**Validation**
- Kibana accessible on `http://10.10.10.2:5601`

---

## Fleet Server health validation
**Goal**
Confirm Fleet Server is listening and healthy before enrolling endpoints.

**Commands**
- `sudo ss -lntp | grep 8220`
- `curl -k https://10.10.10.2:8220/api/status`

**Expected**
- `{"name":"fleet-server","status":"HEALTHY"}`

---

## Windows agent install issues
**Common symptom**
- Install returned: “already installed at C:\Program Files\Elastic\Agent”

**Meaning**
Agent was already installed. No reinstall needed.

**Validation (Admin PowerShell)**
- `& "C:\Program Files\Elastic\Agent\elastic-agent.exe" status`
