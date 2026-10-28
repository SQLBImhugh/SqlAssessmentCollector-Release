# SQL Assessment Collector

Download location for the SQL Assessment Collector package. The collector runs the
`Az.DataMigration` assessment, performance collection and SKU sizing against SQL Server
instances as a scheduled task, and packages the results to send back.

This repository holds only the release. Instructions are in `RUNBOOK.html` and `README.txt`
inside the package.

## Download and extract

Run in Windows PowerShell 5.1 or PowerShell 7 on the server that will run the collector.
Change `C:\SqlAssessment` to the folder you want.

```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$zip = Join-Path $env:TEMP 'SQLAssessmentCollector.zip'
Invoke-WebRequest -Uri 'https://github.com/SQLBImhugh/SqlAssessmentCollector-Release/releases/latest/download/SQLAssessmentCollector.zip' -OutFile $zip -UseBasicParsing
(Get-FileHash $zip -Algorithm SHA256).Hash
Expand-Archive -LiteralPath $zip -DestinationPath 'C:\SqlAssessment' -Force
```

Compare the printed hash with the SHA-256 in the release notes, or in
`SQLAssessmentCollector.zip.sha256` attached to the release. Then open
`C:\SqlAssessment\RUNBOOK.html`.

- The TLS 1.2 line is needed on Windows Server 2016, where Windows PowerShell 5.1 does not
  use TLS 1.2 by default.
- The download goes to `github.com` and is redirected to a GitHub download host
  (`*.githubusercontent.com`). A proxy or firewall must allow both.

## Downloading with a browser

Windows marks a file downloaded with a browser, Outlook or Teams as coming from the
internet. Extracting a marked ZIP with File Explorer marks every extracted file, and
Windows then shows a security warning or asks to unblock each file.

Files downloaded with `Invoke-WebRequest` and extracted with `Expand-Archive`, as above,
are not marked. If you downloaded the ZIP with a browser, either:

- right-click the ZIP, select **Properties**, tick **Unblock**, select **OK**, then extract; or
- extract it with `Expand-Archive` instead of File Explorer.

If the files are already extracted, run this once in the package folder:

```powershell
Get-ChildItem -Recurse | Unblock-File
```

## Releases

Releases are tagged by date (`vYYYY.MM.DD`). Same-day revisions append `.1`, `.2`,
and so on. Every release includes the package and its matching hash; UI updates
can also include a screenshot referenced in the release notes.

| Asset | Content |
|---|---|
| `SQLAssessmentCollector.zip` | The package: the launchers and PowerShell scripts, `README.txt`, `RUNBOOK.html` and the sample `DMS-INPUT-FILE.csv` |
| `SQLAssessmentCollector.zip.sha256` | The ZIP's SHA-256 hash |
| `advanced-options-*.png` (when included) | Screenshot of the release's Setup controls |

The link `releases/latest/download/SQLAssessmentCollector.zip` always returns the newest
release.
