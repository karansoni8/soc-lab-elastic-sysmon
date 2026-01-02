# Detection 03: Windows Service Created (Tuned)

## Goal
Detect suspicious creation of new Windows services, focusing on attacker-like service installs (persistence / execution).

## Data sources
Two common sources:
- Windows System log event: 7045 (A service was installed in the system)
- Sysmon can also show related process creation, but 7045 is the cleanest signal

## KQL (System 7045 + suspicious ImagePath)
```kql
host.os.type: windows and event.code: 7045 and
(
  winlog.event_data.ImagePath:("*\\Users\\*" or "*\\AppData\\*" or "*\\Temp\\*" or "*\\ProgramData\\*") or
  winlog.event_data.ImagePath:(*"powershell"* or *"cmd.exe"* or *"rundll32"* or *"mshta"* or *"wscript"* or *"cscript"*)
)
```
## Attackers frequently create services to run payloads with high privileges and persistence. Suspicious service binary paths and LOLBins are strong indicators.

- Investigation pivots
- winlog.event_data.ServiceName
- winlog.event_data.ImagePath
- user.name / winlog.event_data.AccountName
- process that created it (pivot around the same timestamp)

## Safe test (Admin PowerShell)
Create a test service:
```
sc.exe create SOC-LAB-SVC binPath= "C:\Windows\System32\cmd.exe /c exit" start= demand
```
Delete it
```
sc.exe delete SOC-LAB-SVC
```

## Tuning notes

This rule is intentionally filtered to avoid alert fatigue. If your environment does not populate winlog.event_data.ImagePath, use the message field or the available service path field instead.
