# AutoHotKey example

```autohotkey
#^+q::Run, "C:\Windows\system32\rundll32.exe" sysdm.cpl`,EditEnvironmentVariables
#^p::Run "c:\app\processhacker\ProcessHacker.exe"
#^3::Run "C:\Program Files\Notepad3\Notepad3.exe"
#^u::Run "C:\Users\zte\AppData\Local\Programs\Microsoft VS Code\Code.exe"
#^t::Run "C:\Users\zte\AppData\Local\Microsoft\WindowsApps\wt.exe"
#^c::Run "C:\app\ConEmu\ConEmu64.exe" -reuse -run "{cmd}" -new_console
#^g::Run "C:\app\ConEmu\ConEmu64.exe" -reuse -run "{Git Bash}" -new_console
#^f::Run "C:\app\ConEmu\ConEmu64.exe" -reuse -run "{Far Manager::Far 3.0 x64}" -new_console:P:"^<SolarMe^>"
#^+a::Run "C:\app\ConEmu\ConEmu64.exe" -reuse -run "{Shells::cmd (Admin)}" -new_console

SC1F65::
MsgBox
return

::lipsumka::Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#^+SC056:: ; press control+r to reload
  Msgbox, 4,,Do you really want to reload this script?
  IfMsgBox Yes
    Reload
  return
```
