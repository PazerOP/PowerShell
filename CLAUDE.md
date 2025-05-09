# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## PowerShell Repository Overview

PowerShell is a cross-platform automation and configuration tool consisting of a command-line shell, scripting language, and framework for processing cmdlets. This repository contains the source code for PowerShell 7+, which is different from Windows PowerShell 5.1.

## Build Commands

### Bootstrap & Build

```powershell
# Import the build module
Import-Module ./build.psm1

# Bootstrap the environment (install dependencies)
Start-PSBootstrap

# Build PowerShell
Start-PSBuild -UseNuGetOrg

# Run PowerShell from build output
& (Get-PSOutput)
```

### Platform-Specific Build Options

```powershell
# Clean build with module restoration (Windows)
Start-PSBuild -Clean -PSModuleRestore -UseNuGetOrg

# Fix "Too many open files" error on macOS if needed
ulimit -n 2048
```

## Testing Commands

```powershell
# Run all Pester tests
Start-PSPester -UseNuGetOrg

# Run specific test path
Start-PSPester -Path test/powershell/engine/Api

# Run specific test file
Start-PSPester -Path test/powershell/engine/Api/XmlAdapter.Tests.ps1

# Run xUnit tests
Start-PSxUnit
```

## Test Classifications (Tags)

- `CI`: Tests run during PR validation
- `Feature`: Larger tests run daily
- `Scenario`: Integration tests run daily
- `RequireAdminOnWindows`: Tests requiring admin privileges on Windows
- `RequireSudoOnUnix`: Tests requiring sudo on Unix systems

## Common Development Tasks

```powershell
# Update dependencies
Start-PSBuild -Restore

# Regenerate resources
Start-PSBuild -ResGen

# Regenerate type catalog
Start-PSBuild -TypeGen

# Start a development instance of PowerShell
Start-DevPowerShell
```

## Project Architecture

### Key Source Directories

- `/src/powershell-win-core/`: Windows-specific implementation
- `/src/powershell-unix/`: Linux/macOS implementation
- `/src/System.Management.Automation/`: Core PowerShell engine
  - `/engine/`: Core runtime components
  - `/help/`: Help subsystem
  - `/logging/`: Logging infrastructure
  - `/namespaces/`: Provider implementations
- `/src/Microsoft.PowerShell.Commands.*`: Cmdlet implementations

### Build System

The build system is based on .NET SDK and orchestrated through the `build.psm1` PowerShell module. Key components:
- `Start-PSBootstrap`: Installs dependencies for building
- `Start-PSBuild`: Builds PowerShell from source
- `Start-PSPester`: Runs Pester tests
- `Start-PSxUnit`: Runs xUnit tests
- `Get-PSOutput`: Gets the path to the built PowerShell executable

The build process includes resource generation (`Start-ResGen`) and type catalog generation (`Start-TypeGen`).

## Building Documentation Links

- Windows: https://github.com/PowerShell/PowerShell/tree/master/docs/building/windows-core.md
- Linux: https://github.com/PowerShell/PowerShell/tree/master/docs/building/linux.md
- macOS: https://github.com/PowerShell/PowerShell/tree/master/docs/building/macos.md