# MS17-010 Eternal Romance PoC
### Originally from github.com/worawit/MS17-010
Updated SMB connection and the ability to generate an meterpreter on the fly to exectue as a service upon successful exploitation, be aware this will most likely not work on boxes with EPP.

Requires access to a named pipe to successfully exploit however don't feel the need to add one, it'll automatically check for the most common and check if available :)
Always remember to set up handlers kids ! **use exploit/multi/handler/**
