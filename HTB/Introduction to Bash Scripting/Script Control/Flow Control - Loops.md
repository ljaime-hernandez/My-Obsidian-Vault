The control of the flow of our scripts is essential. We have already learned about the `if-else` conditions, which are also part of flow control. After all, we want our script to work quickly and efficiently, and for this, we can use other components to increase efficiency and allow error-free processing. Each control structure is either a `branch` or a `loop`. Logical expressions of boolean values usually control the execution of a control structure. These control structures include:

-   Branches:
    -   `If-Else` Conditions
    -   `Case` Statements
-   Loops:
    -   `For` Loops
    -   `While` Loops
    -   `Until` Loops

## For Loops

Let us start with the `For` loops. The `For` loop is executed on each pass for precisely one parameter, which the shell takes from a list, calculates from an increment, or takes from another data source. The for loop runs as long as it finds corresponding data. This type of loop can be structured and defined in different ways. For example, the for loops are often used when we need to work with many different values from an array. This can be used to scan different hosts or ports. We can also use it to execute specific commands for known ports and their services to speed up our enumeration process. The syntax for this can be as follows:

#### Syntax - Examples

```bash
for variable in 1 2 3 4
do
	echo $variable
done
```

```bash
for variable in file1 file2 file3
do
	echo $variable
done
```

```bash
for ip in "10.10.10.170 10.10.10.174 10.10.10.175"
do
	ping -c 1 $ip
done
```

Of course, we can also write these commands in a single line. Such a command would look like this:

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ for ip in 10.10.10.170 10.10.10.174;do ping -c 1 $ip;done

PING 10.10.10.170 (10.10.10.170): 56 data bytes
64 bytes from 10.10.10.170: icmp_seq=0 ttl=63 time=42.106 ms

--- 10.10.10.170 ping statistics ---
1 packets transmitted, 1 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 42.106/42.106/42.106/0.000 ms
PING 10.10.10.174 (10.10.10.174): 56 data bytes
64 bytes from 10.10.10.174: icmp_seq=0 ttl=63 time=45.700 ms

--- 10.10.10.174 ping statistics ---
1 packets transmitted, 1 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 45.700/45.700/45.700/0.000 ms
```

Let us have another look at our `CIDR.sh` script. We have added several for loops to the script, but let us stick with this little code section.

#### CIDR.sh

```bash
<SNIP>

# Identify Network range for the specified IP address(es)
function network_range {
	for ip in $ipaddr
	do
		netrange=$(whois $ip | grep "NetRange\|CIDR" | tee -a CIDR.txt)
		cidr=$(whois $ip | grep "CIDR" | awk '{print $2}')
		cidr_ips=$(prips $cidr)
		echo -e "\nNetRange for $ip:"
		echo -e "$netrange"
	done
}

<SNIP>
```

As in the previous example, for each IP address from the array "`ipaddr`" we make a "`whois`" request, whose output is filtered for "`NetRange`" and "`CIDR`." This helps us to determine which address range our target is located in. We can use this information to search for additional hosts during a penetration test, `if approved by the client`. The results that we receive are displayed accordingly and stored in the file "`CIDR.txt`."

## While Loops

The `while` loop is conceptually simple and follows the following principle:

-   A statement is executed as long as a condition is fulfilled (`true`).

We can also combine loops and merge their execution with different values. It is important to note that the excessive combination of several loops in each other can make the code very unclear and lead to errors that can be hard to find and follow. Such a combination can look like in our `CIDR.sh` script.

#### CIDR.sh

```bash
<SNIP>
		stat=1
		while [ $stat -eq 1 ]
		do
			ping -c 2 $host > /dev/null 2>&1
			if [ $? -eq 0 ]
			then
				echo "$host is up."
				((stat--))
				((hosts_up++))
				((hosts_total++))
			else
				echo "$host is down."
				((stat--))
				((hosts_total++))
			fi
		done
<SNIP>
```

The `while` loops also work with conditions like `if-else`. A while loop needs some sort of a counter to orientate itself when it has to stop executing the commands it contains. Otherwise, this leads to an endless loop. Such a counter can be a variable that we have declared with a specific value or a boolean value. `While` loops run while the boolean value is "`True`". Besides the counter, we can also use the command "`break`," which interrupts the loop when reaching this command like in the following example:

#### WhileBreaker.sh

```bash
#!/bin/bash

counter=0

while [ $counter -lt 10 ]
do
  # Increase $counter by 1
  ((counter++))
  echo "Counter: $counter"

  if [ $counter == 2 ]
  then
    continue
  elif [ $counter == 4 ]
  then
    break
  fi
done
```

#### WhileBreaker.sh

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ ./WhileBreaker.sh

Counter: 1
Counter: 2
Counter: 3
Counter: 4
```

## Until Loops

There is also the `until` loop, which is relatively rare. Nevertheless, the `until` loop works precisely like the `while` loop, but with the difference:

-   The code inside a `until` loop is executed as long as the particular condition is `false`.

The other way is to let the loop run until the desired value is reached. The "`until`" loops are very well suited for this. This type of loop works similarly to the "`while`" loop but, as already mentioned, with the difference that it runs until the boolean value is "`False`."

#### Until.sh

```bash
#!/bin/bash

counter=0

until [ $counter -eq 10 ]
do
  # Increase $counter by 1
  ((counter++))
  echo "Counter: $counter"
done
```

#### Until.sh

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ ./Until.sh

Counter: 1
Counter: 2
Counter: 3
Counter: 4
Counter: 5
Counter: 6
Counter: 7
Counter: 8
Counter: 9
Counter: 10
```

## Exercise Script

```bash
#!/bin/bash

# Decrypt function
function decrypt {
	MzSaas7k=$(echo $hash | sed 's/988sn1/83unasa/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/4d298d/9999/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/3i8dqos82/873h4d/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/4n9Ls/20X/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/912oijs01/i7gg/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/k32jx0aa/n391s/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/nI72n/YzF1/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/82ns71n/2d49/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/JGcms1a/zIm12/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/MS9/4SIs/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/Ymxj00Ims/Uso18/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/sSi8Lm/Mit/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/9su2n/43n92ka/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/ggf3iunds/dn3i8/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/uBz/TT0K/g')

	flag=$(echo $MzSaas7k | base64 -d | openssl enc -aes-128-cbc -a -d -salt -pass pass:$salt)
}

# Variables
var="9M"
salt=""
hash="VTJGc2RHVmtYMTl2ZnYyNTdUeERVRnBtQWVGNmFWWVUySG1wTXNmRi9rQT0K"

# Base64 Encoding Example:
#        $ echo "Some Text" | base64

# <- For-Loop here

# Check if $salt is empty
if [[ ! -z "$salt" ]]
then
	decrypt
	echo $flag
else
	exit 1
fi
```


Create a "For" loop that encodes the variable "var" 28 times in "base64". The number of characters in the 28th hash is the value that must be assigned to the "salt" variable.

`HTBL00p5r0x`

Ans:
```bash
var="9M"
salt=""
hash="VTJGc2RHVmtYMTl2ZnYyNTdUeERVRnBtQWVGNmFWWVUySG1wTXNmRi9rQT0K"

for counter in {1..28}
do
   var=$(echo $var | base64)
done

salt=$(echo $var | wc -c)

if [[ ! -z "$salt" ]]
then
    decrypt
    echo $flag
else
    exit 1
fi
```

* line 5: made a for loop which will iterate 28 times as requested
* line 7: `var` string will be encoded on base64 each time the loop comes back to itself
* line 10: assign the number of characters in the `var` encoded string to `salt` variable
* line 12: checks if `salt` variable is null, if its not null then it will access the condition
* line 14: runs `decrypt` function
* line 15: prints the flag produced in the `decrypt` function