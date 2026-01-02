# SOC Detection Lab (Elastic SIEM + Fleet + Sysmon)

I built a small SOC lab that collects endpoint telemetry from Windows using Sysmon, ships it through Elastic Agent + Fleet Server, and generates security alerts using custom detections. I validated detections end-to-end and documented investigation workflow (alert → triage → incident report).

## What this demonstrates
- Telemetry pipeline build: endpoint → Fleet → Elasticsearch → Kibana Security
- Endpoint visibility with Sysmon (process creation + command line + parent process)
- Detection engineering (KQL rules + tuning to reduce noise)
- Alert triage (reading alert fields/JSON, investigation pivots, conclusions)
- Troubleshooting (logs, ports, services, container lifecycle)

## Architecture
See: [ARCHITECTURE.md](ARCHITECTURE.md)

### Diagram (Mermaid)
```mermaid
flowchart LR
  W[Windows 10\nSysmon + Elastic Agent\n10.10.10.3] -->|Enroll / Policy| F[Fleet Server\n10.10.10.2:8220]
  F -->|Ingest| E[Elasticsearch\n10.10.10.2:9200]
  K[Kibana Security\n10.10.10.2:5601] <-->|Search / Visualize| E
  K --> A[Detections + Alerts]
```

## Detections
- Encoded PowerShell execution: [detections/01-encoded-powershell.md](detections/01-encoded-powershell.md)
- Local admin group change: [detections/02-local-admin-change.md](detections/02-local-admin-change.md)
- Windows service created (tuned): [detections/03-service-created.md](detections/03-service-created.md)

## Incident reports
- INC-0001 (Encoded PowerShell triage): [incidents/INC-0001-encoded-powershell.md](incidents/INC-0001-encoded-powershell.md)

## Troubleshooting notes
See: [TROUBLESHOOTING.md](TROUBLESHOOTING.md)

## Lab notes
This is a lab environment (self-signed certs, `--insecure` used for enrollment). The focus is detection + investigation workflow rather than production hardening.
