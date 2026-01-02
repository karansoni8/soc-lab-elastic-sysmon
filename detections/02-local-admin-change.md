# Detection 02: Local Admin Group Change

## Goal
Detect attempts to add a user to privileged groups (local Administrators). This is a common privilege escalation step.

## Data sources
Two common ways (depends on what logs are enabled in the lab):
- Windows Security Event Logs (recommended): 4728 / 4732 / 4756 (group membership changes)
- Alternative (lab-friendly): process-based detection of `net.exe` / `net1.exe` localgroup usage

## Option A (Security logs) — if available
### KQL
```kql
host.os.type: windows and event.code: (4728 or 4732 or 4756 or 4735)
````
## Why it matters

Adding accounts to local admin groups enables persistence and full control of the host.

Safe test (Admin PowerShell)
```powershell
net user soclabtest P@ssw0rd! /add
net localgroup Administrators soclabtest /add
```
cleanup 
````
net localgroup Administrators soclabtest /delete
net user soclabtest /delete
````

## Note
Process-based detection is noisier, but it’s useful in labs where Security auditing isn’t fully enabled.


