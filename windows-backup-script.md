# Windows backup script example

## About

This script backs up some app settings used by me to a directory. It depends on `7za` program, which creates zip files.

```bat
rem Backup
set bck="C:\backup"
cd %bck%

rem Backup app-bin
C:\app\bin\7za.exe a %bck%\app-bin.7z c:\app\bin\
rem .aws
C:\app\bin\7za.exe a %bck%\xsak-aws.7z %userprofile%\.aws "-xr!*.json.docs" "-xr!*.json"
rem .ssh
C:\app\bin\7za.exe a %bck%\xsak-ssh.7z %userprofile%\.ssh
rem %userprofile%\ dotfiles
C:\app\bin\7za.exe a %bck%\xsak-dotfiles.7z %userprofile%\.bash* %userprofile%\.git* %userprofile%\.profile* %userprofile%\.vim* %userprofile%\*.bat %userprofile%\pip\
rem Vivaldi
C:\app\bin\7za.exe a %bck%\xsak-vivaldi.7z "%localappdata%\Vivaldi\User Data" -xr!Cache -xr!"Media Cache" -xr!GPUCache
rem Notepad3
C:\app\bin\7za.exe a %bck%\xsak-notepad3.7z "%appdata%\Rizonesoft\*"
rem Notepad++
C:\app\bin\7za.exe a %bck%\Notepadplusplus.7z "C:\Program Files (x86)\Notepad++\"
C:\app\bin\7za.exe a %bck%\xsak-Notepadplusplus.7z "%appdata%\Notepad++\"
rem Atom
C:\app\bin\7za.exe a %bck%\xsak-atom.7z %userprofile%\.atom
call %userprofile%\AppData\Local\atom\bin\apm.cmd list --installed --bare > %bck%\atom-pkg.list
rem AutoHotKey
copy %userprofile%\xsak.ahk %bck%\xsak.ahk
rem VSCode
C:\app\bin\7za.exe a %bck%\xsak-vscode-ext.7z %userprofile%\.vscode
C:\app\bin\7za.exe a %bck%\xsak-vscode.7z %appdata%\Code
rem Start Menu
C:\app\bin\7za.exe a %bck%\xsak-startmenu.7z "%appdata%\Microsoft\Windows\Start Menu\Programs\"
rem ConEmu
C:\app\bin\7za.exe a %bck%\xsak-conemu.7z %appdata%\ConEmu.xml
rem Sublime Text
rem C:\app\bin\7za.exe a %bck%\xsak-sublime.7z "C:\app\SublimeText"
rem FAR manager
"C:\Program Files\Far Manager\Far.exe" /export %bck%\FAR.cfg
C:\app\bin\7za.exe a %bck%\xsak-far.7z "C:\Program Files\Far Manager\"
C:\app\bin\7za.exe a %bck%\xsak-far-profile.7z "%APPDATA%\Far Manager"
rem Keypirinha
C:\app\bin\7za.exe a %bck%\xsak-keypirinha-roaming.7z %appdata%\Keypirinha
C:\app\bin\7za.exe a %bck%\xsak-keypirinha-local.7z %localappdata%\Keypirinha
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
