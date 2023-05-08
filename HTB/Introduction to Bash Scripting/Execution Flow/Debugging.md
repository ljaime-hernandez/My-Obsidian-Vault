Bash gives us an excellent opportunity to find, track, and fix errors in our code. The term `debugging` can have many different meanings. Nevertheless, [Bash debugging](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_02_03.html) is the process of removing errors (bugs) from our code. Debugging can be performed in many different ways. For example, we can use our code for debugging to check for typos, or we can use it for code analysis to track them and determine why specific errors occur.

This process is also used to find vulnerabilities in programs. For example, we can try to cause errors using different input types and track their handling in the CPU through the assembler, which may provide a way to manipulate the handling of these errors to insert our own code and force the system to execute it. This topic will be covered and discussed in detail in other modules. Bash allows us to debug our code by using the "`-x`" (`xtrace`) and "`-v`" options. Now let us see an example with our `CIDR.sh` script.

#### CIDR.sh - Debugging

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ bash -x CIDR.sh

+ '[' 0 -eq 0 ']'
+ echo -e 'You need to specify the target domain.\n'
You need to specify the target domain.

+ echo -e Usage:
Usage:
+ echo -e '\tCIDR.sh <domain>'
	CIDR.sh <domain>
+ exit 1
```

Here Bash shows us precisely which function or command was executed with which values. This is indicated by the plus sign (`+`) at the beginning of the line. If we want to see all the code for a particular function, we can set the "`-v`" option that displays the output in more detail.

#### CIDR.sh - Verbose Debugging

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ bash -x -v CIDR.sh

#!/bin/bash

# Check for given argument
if [ $# -eq 0 ]
then
	echo -e "You need to specify the target domain.\n"
	echo -e "Usage:"
	echo -e "\t$0 <domain>"
	exit 1
else
	domain=$1
fi
+ '[' 0 -eq 0 ']'
+ echo -e 'You need to specify the target domain.\n'
You need to specify the target domain.

+ echo -e Usage:
Usage:
+ echo -e '\tCIDR.sh <domain>'
	CIDR.sh <domain>
+ exit 1
```

In comparison to normal debugging, we see the entire code section that has been processed so far and then the individual steps that have been taken.