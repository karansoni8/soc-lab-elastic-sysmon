# Detection 01: Suspicious PowerShell - EncodedCommand

## Goal
Detect PowerShell executions that use Base64/encoded command patterns often used for obfuscation.

## Data source
Sysmon Event ID 1 (Process Create) via `Microsoft-Windows-Sysmon/Operational`

## KQL
```kql
host.os.type: windows and event.category: process and
(process.name: "powershell.exe" or process.name: "pwsh.exe") and
process.command_line: (*EncodedCommand* or *-enc* or * -e * or *FromBase64String*)
```
## Why it matters
Encoded PowerShell is commonly used by attackers to hide payloads and evade basic detections.

## Investigation pivots

- host.name, host.ip
- user.name
- process.command_line (decode if needed)
- process.parent.name (Office/browser spawning PowerShell is high risk)
- process.hash.sha256 (search prevalence across hosts)

## Safe test
```powershell
powershell -NoP -EncodedCommand SQBleABpAHQ=
```
