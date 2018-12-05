# MS17-010 Eternal Romance PoC
### Originally from github.com/worawit/MS17-010
Updated SMB connection and the ability to generate an meterpreter on the fly to execute as a service upon successful exploitation, be aware this will most likely not work on boxes with EPP.

Requires access to a named pipe to successfully exploit however don't feel the need to add one, it'll automatically check for the most common and check if available :)
Always remember to set up handlers kids ! **use exploit/multi/handler/**

If you get import errors you're probably missing impacket ! **pip install impacket**

**Exploitable On:**
- Windows 2016 x64
- Windows 10 Pro Build 10240 x64
- Windows 2012 R2 x64
- Windows 8.1 x64
- Windows 2008 R2 SP1 x64
- Windows 7 SP1 x64
- Windows 2008 SP1 x64
- Windows 2003 R2 SP2 x64
- Windows XP SP2 x64
- Windows 8.1 x86
- Windows 7 SP1 x86
- Windows 2008 SP1 x86
- Windows 2003 SP2 x86
- Windows XP SP3 x86
- Windows 2000 SP4 x86
