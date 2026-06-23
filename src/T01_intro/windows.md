# Windows Setup (PowerShell 7 + Chocolatey)

## 1. Set Execution Policy in PowerShell 5

> PowerShell checks the execution policy before running a script. If the script doesn’t meet the requirements (like source or signature), PowerShell blocks its execution. We need to allow scripts to be run.

- Open `PowerShell` with administrative right
- If you use Win 32
  - `New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force`

- `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine`

## 2. Install `choco`

> Choco is the command-line tool for Chocolatey, a popular package manager for Windows that automates software installation, updating, and removal using simple commands.

- `Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))`

## 3. Install PowerShell 7

- Open Command Prompt (`cmd`) as an administrator.
- `choco install powershell-core`

## 4. Set PowerShell 7 as Default in Windows Terminal

- Open Windows Terminal (you can search for it in the Start menu).
  -Click the dropdown arrow next to the tabs, then select Settings.
- Under Startup (or General), look for Default profile.
- Choose `PowerShell 7`
  - It might be listed as `PowerShell` with a higher version number, like `pwsh`.
- Click `save`.

## 5. (Optional) Install software

- Close and reopen Terminal with administrative right
- `choco install vscode -y`
- `choco install dbeaver -y`
- `choco install git -y`
- `choco install insomnia-rest-api-client`
