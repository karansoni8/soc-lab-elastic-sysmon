# Architecture

## Environment
- Ubuntu (Elastic Stack + Fleet Server): 10.10.10.2
- Windows (Endpoint): 10.10.10.3
- Kali (optional): 10.10.10.4

## Ports
- 5601: Kibana UI
- 9200: Elasticsearch API
- 8220: Fleet Server

## Data flow
Windows Sysmon → Elastic Agent → Fleet Server → Elasticsearch → Kibana Security → Detection Rules → Alerts

## Diagram (Mermaid)
```mermaid
flowchart LR
  W[Windows 10\nSysmon + Elastic Agent\n10.10.10.3] -->|Enroll / Policy| F[Fleet Server\n10.10.10.2:8220]
  F -->|Ingest| E[Elasticsearch\n10.10.10.2:9200]
  K[Kibana Security\n10.10.10.2:5601] <-->|Search / Visualize| E
  K --> A[Detections + Alerts]
