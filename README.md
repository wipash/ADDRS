# ADDRS - Azure Data Driven Right Sizing
_Automatically right sizes any Azure VM based on Log Analytics telemetry data to the optimal size based on cpu/mem, performance rating and cost._

Forked from [Jos Lieben](https://gitlab.com/Lieben/assortedFunctions/-/tree/master/ADDRS)

<br/>

---

## Original documentation
https://www.lieben.nu/liebensraum/2022/05/automatic-modular-rightsizing-of-azure-vms-with-special-focus-on-azure-virtual-desktop/

## License
https://www.lieben.nu/liebensraum/commercial-use/

<br/>

---

## Changes
- Add more info to report (current and required CPU and RAM, and perf scores)
- Remove the `-Domain` option, which appended the domain to the end of the VM name in the Log Analytics query. Instead, query Log Analytics for `Computer startswith` to allow for mixes of domain-joined and non-domain-joined machines in the same resource group.
- Query Log Analytics for `CounterName startswith`, to cover counter names that differ between Windows and Linux guests
- Append resource group name to reports, so multiple reports for different resource groups can be run simultaneously
- Auto-format files using VS Code PowerShell extension

<br/>

---

## Example
To report on all VMs in your subscription
```powershell

$LogAnalyticsWorkspace = "2d1404e1-0e9a-4589-93f3-b7e708c5b0a4"
$Region = "westus"
$VMs = Get-AzVM
$RGs = $vms.ResourceGroupName | Select-Object -unique


$RGs | ForEach-Object {
    Write-Host -ForegroundColor Green $_
    set-rsgRightSize -workspaceId $LogAnalyticsWorkspace -region $Region -targetRSG $_ -WhatIf -Report
}
```
