classes: wide
---
# Windows backup script example

## About

This script backs up some app settings used by me to a specified directory. It uses the `7za` program, which creates zip files.

```bat
rem Backup
set bck="C:\Users\MyUser\OneDrive\Documents\backup"
cd %bck%
set sevenz="C:\app\bin\7za.exe"

rem Backup app-bin
%sevenz% a %bck%\app-bin.7z c:\app\bin\
rem .aws
%sevenz% a %bck%\xsak-aws.7z %userprofile%\.aws "-xr!*.json.docs" "-xr!*.json"
rem .ssh
%sevenz% a %bck%\xsak-ssh.7z %userprofile%\.ssh
rem %userprofile%\ dotfiles
%sevenz% a %bck%\xsak-dotfiles.7z %userprofile%\.bash* %userprofile%\.git* %userprofile%\.profile* %userprofile%\.vim* %userprofile%\*.bat %userprofile%\pip\
rem Vivaldi
%sevenz% a %bck%\xsak-vivaldi.7z "%localappdata%\Vivaldi\User Data" -xr!Cache -xr!"Media Cache" -xr!GPUCache
rem Notepad3
%sevenz% a %bck%\xsak-notepad3.7z "%appdata%\Rizonesoft\*"
rem Notepad++
%sevenz% a %bck%\Notepadplusplus.7z "C:\Program Files (x86)\Notepad++\"
%sevenz% a %bck%\xsak-Notepadplusplus.7z "%appdata%\Notepad++\"
rem Atom
%sevenz% a %bck%\xsak-atom.7z %userprofile%\.atom
call %userprofile%\AppData\Local\atom\bin\apm.cmd list --installed --bare > %bck%\atom-pkg.list
rem AutoHotKey
copy /Y %userprofile%\xsak.ahk %bck%\xsak.ahk
rem VSCode
%sevenz% a %bck%\xsak-vscode-ext.7z %userprofile%\.vscode
%sevenz% a %bck%\xsak-vscode.7z %appdata%\Code
rem Start Menu
%sevenz% a %bck%\xsak-startmenu.7z "%appdata%\Microsoft\Windows\Start Menu\Programs\"
rem ConEmu
%sevenz% a %bck%\xsak-conemu.7z C:\app\ConEMU\ConEmu.xml
rem Sublime Text
rem %sevenz% a %bck%\xsak-sublime.7z "C:\app\SublimeText"
rem FAR manager
"C:\Program Files\Far Manager\Far.exe" /export %bck%\FAR.cfg
%sevenz% a %bck%\xsak-far.7z "C:\Program Files\Far Manager\"
%sevenz% a %bck%\xsak-far-profile.7z "%APPDATA%\Far Manager"
rem Keypirinha
%sevenz% a %bck%\xsak-keypirinha-roaming.7z %appdata%\Keypirinha
%sevenz% a %bck%\xsak-keypirinha-local.7z %localappdata%\Keypirinha
rem PuTTY
C:\Windows\regedit /e putty-registry.reg "HKEY_CURRENT_USER\Software\Simontatham"
rem Choco
choco list --localonly > %bck%\choco-list.txt
rem mRemoteNG
copy %appdata%\mRemoteNG\confCons.xml %bck%\confCons.xml

```

## Scheduling

To have this script run every day at 11:52 the following command creates a Task Scheduler task under xsak folder named daily_backup:

```bat
schtasks.exe /create /tn "xsak\daily_backup" /sc daily /st 11:52  /tr "cmd /c \"C:\backupscriptdir\backup.bat\""
```
