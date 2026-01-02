# INC-0001: Encoded PowerShell Alert (Lab Validation)

## Summary
A detection alert triggered for PowerShell using `-EncodedCommand`, a common obfuscation technique. This was a controlled lab test to validate the telemetry → detection → alert pipeline.

## Alert metadata
- Rule: Suspicious PowerShell - EncodedCommand
- Severity: Medium
- Risk score: 50
- Provider: Microsoft-Windows-Sysmon
- Dataset: windows.sysmon_operational
- Event ID: 1 (Sysmon Process Create)

## Key evidence (from alert JSON)
- Alert time (UTC): 2026-01-01T22:56:16.335Z
- Host: desktop-s8985rp
- Host IP: 10.10.10.3
- User: Karan

### Process
- Name: powershell.exe
- Path: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
- Command line:
  `"C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -NoP -EncodedCommand SQBleABpAHQ=`

### Parent process
- Name: powershell.exe
- Path: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe

## Triage reasoning
- Encoded PowerShell is a common attacker technique; rule correctly flagged the behavior.
- Context showed manual execution in a lab environment (parent process = PowerShell, expected host/user).
- No suspicious follow-on behavior was observed for this validation test.

## Disposition
- Detection outcome: True positive (rule worked)
- Activity classification: Benign (authorized lab simulation)

## Recommended actions
- Keep detection enabled.
- In production, tune with exceptions for known admin scripts and add higher confidence checks (parent process, signed script paths, workstation vs server context).
