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
