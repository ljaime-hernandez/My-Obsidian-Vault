## Note Taking

There are five different main types of information that need to be noted down:

1.  Newly discovered information   
2.  Ideas for further tests and processing
3.  Scan results
4.  Logging
5.  Screenshots

### Discovered Information

By discovered information we mean, general information, such as new IP addresses, usernames, passwords, source code, etc., that we identified and are related to the penetration testing engagement and process. This is information that we can use against our target company. We often obtain such information through OSINT, active scans, and manual analysis of the given information resources and services.

### Processing

We will receive a lot of different information during our penetration testing that will require us to adapt our approach. These results may give us ideas for subsequent steps we can take, and other vulnerabilities or misconfigurations may be forgotten or overlooked. Therefore, we should get in the habit of noting down everything we see that should be investigated as part of the assessment. Notion.so, Obsidian and Xmind are very suitable for this.

[Obsidian](https://obsidian.md/) is a powerful knowledge base that works on top of a local folder of plain text Markdown files.

### Results

Only through practice, our eyes can be trained to recognize the essential small fragments of information. Nevertheless, we should keep all information and results not to miss something meaningful and because a piece of information may prove helpful later in the engagement. For this, we can use [GhostWriter](https://github.com/GhostManager/Ghostwriter) or [Pwndoc](https://github.com/pwndoc/pwndoc). These allow us to generate our documentation and have a clear overview of the steps we have taken.

### Logging

Logging is essential for both documentation and our protection. If third parties attack the company during our penetration test and damage occurs, we can prove that the damage did not result from our activities. For this, we can use the tools `script` and `date`. `Date` can be used to display the exact date and time of each command in our command line. With the help of `script`, every command and the subsequent result is saved in a background file. To display the date and time, we can replace the `PS1` variable in our `.bashrc` file with the following content.

Code: bash
```bash
PS1="\[\033[1;32m\]\342\224\200\$([[ \$(/opt/vpnbash.sh) == *\"10.\"* ]] && echo \"[\[\033[1;34m\]\$(/opt/vpnserver.sh)\[\033[1;32m\]]\342\224\200[\[\033[1;37m\]\$(/opt/vpnbash.sh)\[\033[1;32m\]]\342\224\200\")[\[\033[1;37m\]\u\[\033[01;32m\]@\[\033[01;34m\]\h\[\033[1;32m\]]\342\224\200[\[\033[1;37m\]\w\[\033[1;32m\]]\n\[\033[1;32m\]\342\224\224\342\224\200\342\224\200\342\225\274 [\[\e[01;33m\]$(date +%D-%r)\[\e[01;32m\]]\\$ \[\e[0m\]"
```

To start logging with `script` (for Linux) and [Start-Transcript](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.host/start-transcript?view=powershell-7.1) (for Windows), we can use the following command and rename it according to our needs. It is recommended to define a certain format in advance after saving the individual logs. One option is using the format `<date>-<start time>-<name>.log`.

For Script:

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ script 03-21-2021-0200pm-exploitation.log

Luis Miguel Jaime Hernandez@htb[/htb]$ ...SNIP...
Luis Miguel Jaime Hernandez@htb[/htb]$ exit
```

For Start-Transcript:

```powershell-session
C:\> Start-Transcript -Path "C:\Pentesting\03-21-2021-0200pm-exploitation.log"

Transcript started, output file is C:\Pentesting\03-21-2021-0200pm-exploitation.log

C:\> ...SNIP...
C:\> Stop-Transcript
```

There are also terminal emulators, such as [Tmux](https://github.com/tmux/tmux/wiki) and [Terminator](https://terminator-gtk3.readthedocs.io/en/latest/), which allow, among other things, to log all commands and output automatically. If we come across a tool that does not allow us to log the output, we can work with redirections and the program tee. This would look like this:

For Linux Output Redirection:

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ ./custom-tool.py 10.129.28.119 > logs.custom-tool
```

For Linux Output Redirection with tee:

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ ./custom-tool.py 10.129.28.119 | tee -a logs.custom-tool
```

For Windows Output Redirection:

```powershell-session
C:\> .\custom-tool.ps1 10.129.28.119 > logs.custom-tool
```

For Windows Output Redirection with Out-File:

```powershell-session
C:\> .\custom-tool.ps1 10.129.28.119 | Out-File -Append logs.custom-tool
```

### Screenshots

Screenshots serve as a momentary record and represent proof of results obtained, necessary for the Proof-Of-Concept and our documentation. One of the best tools for this is [Flameshot](https://github.com/flameshot-org/flameshot). It has all the essential functions that we need to quickly edit our screenshots without using an additional editing program. We can install it using our APT package manager or via download from Github.

Sometimes, however, we cannot show all the necessary steps in one or more screenshots. We can use an application called [Peek](https://github.com/phw/peek) and create GIFs that record all the required actions for us.


