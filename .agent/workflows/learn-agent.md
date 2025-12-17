---
description: Learn best practices and rules from another agent workspace (Local or GitHub) using /learn-agent.
---

1.  **Identify Source**: Determine if the target is a local directory or a GitHub repository.
2.  **Fetch Rules**: Retrieve the contents of `.agent/rules` from the source.
3.  **Analyze & Apply**: Compare with local rules and propose/apply updates.
4.  **Update Metadata**: If any changes are applied, update `version` (bump patch) and `last_updated` date in the frontmatter of the modified files.

```powershell
param (
    [string]$SourcePath
)

# 1. Setup Source
if (-not $SourcePath) {
    $SourcePath = Read-Host "Enter GitHub URL or Local Path to Agent Workspace"
}

$tempDir = $null
$srcRulesDir = $null
$srcWorkflowsDir = $null

if ($SourcePath -match "^https?://.*github.com.*") {
    Write-Host "Detected GitHub URL. Cloning..." -ForegroundColor Cyan
    $tempDir = Join-Path $env:TEMP "agent-learn-rules-$(Get-Random)"
    git clone --depth 1 $SourcePath $tempDir
    if ($LASTEXITCODE -eq 0) {
        $srcRulesDir = Join-Path $tempDir ".agent\rules"
        $srcWorkflowsDir = Join-Path $tempDir ".agent\workflows"
    } else {
        Write-Error "Failed to clone repository."
        exit
    }
} elseif (Test-Path $SourcePath) {
    if (Test-Path (Join-Path $SourcePath ".agent")) {
        $srcRulesDir = Join-Path $SourcePath ".agent\rules"
        $srcWorkflowsDir = Join-Path $SourcePath ".agent\workflows"
    } else {
        # Try direct subdirs
        $srcRulesDir = Join-Path $SourcePath "rules"
        $srcWorkflowsDir = Join-Path $SourcePath "workflows"
        if (-not (Test-Path $srcRulesDir)) { $srcRulesDir = Join-Path $SourcePath ".agent\rules" }
        if (-not (Test-Path $srcWorkflowsDir)) { $srcWorkflowsDir = Join-Path $SourcePath ".agent\workflows" }
    }
}

# 2. Define Sync Function
function Sync-Directory {
    param (
        [string]$Src,
        [string]$Dest,
        [string]$Label
    )

    if (-not (Test-Path $Src)) {
        Write-Warning "Source $Label directory not found: $Src"
        return
    }

    if (-not (Test-Path $Dest)) {
        New-Item -ItemType Directory -Path $Dest -Force | Out-Null
    }

    $files = Get-ChildItem $Src -Filter "*.md"
    Write-Host "`n=== Syncing $Label ($($files.Count) files) ===" -ForegroundColor Magenta

    foreach ($file in $files) {
        $destFile = Join-Path $Dest $file.Name
        $action = "Skip"

        if (-not (Test-Path $destFile)) {
            Write-Host "[NEW] $($file.Name)" -ForegroundColor Green
            $ans = Read-Host "    Import this new file? (y/n)"
            if ($ans -eq 'y') { $action = "Copy" }
        } else {
            # Compare Content (Simple Hash)
            $srcHash = (Get-FileHash $file.FullName).Hash
            $destHash = (Get-FileHash $destFile).Hash
            
            if ($srcHash -ne $destHash) {
                Write-Host "[MOD] $($file.Name)" -ForegroundColor Yellow
                $ans = Read-Host "    File differs. (o)verwrite, (v)iew diff, (s)kip?"
                if ($ans -eq 'o') { 
                    $action = "Copy" 
                } elseif ($ans -eq 'v') {
                    Write-Host "`n--- SOURCE: $($file.Name) ---" -ForegroundColor Gray
                    Get-Content $file.FullName | Select-Object -First 10
                    Write-Host "...(truncated)..."
                    Write-Host "--- LOCAL: $($file.Name) ---" -ForegroundColor Gray
                    Get-Content $destFile | Select-Object -First 10
                    Write-Host "...(truncated)...`n"
                    
                    $ans2 = Read-Host "    Overwrite after viewing? (y/n)"
                    if ($ans2 -eq 'y') { $action = "Copy" }
                }
            } else {
                 Write-Host "[OK]  $($file.Name)" -ForegroundColor DarkGray
            }
        }

        if ($action -eq "Copy") {
            Copy-Item $file.FullName -Destination $destFile -Force
            Write-Host "    ✅ Updated $($file.Name)" -ForegroundColor Green
        }
    }
}

# 3. Execute Sync
$localRules = ".agent\rules"
$localWorkflows = ".agent\workflows"

Sync-Directory -Src $srcRulesDir -Dest $localRules -Label "Rules"
Sync-Directory -Src $srcWorkflowsDir -Dest $localWorkflows -Label "Workflows"

# 4. Cleanup
if ($tempDir -and (Test-Path $tempDir)) {
    Remove-Item $tempDir -Recurse -Force -ErrorAction SilentlyContinue
}

Write-Host "`nDone." -ForegroundColor Cyan
```
