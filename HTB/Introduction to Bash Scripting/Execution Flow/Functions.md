The bigger our scripts get, the more chaotic they become. If we use the same routines several times in the script, the script's size will increase accordingly. In such cases, `functions` are the solution that improves both the size and the clarity of the script many times. We combine several commands in a block between curly brackets ( `{` ... `}` ) and call them with a function name defined by us with `functions`. Once a function has been defined, it can be called and used again during the script.

`Functions` are an essential part of scripts and programs, as they are used to execute recurring commands for different values and phases of the script or program. Therefore, we do not have to repeat the whole section of code repeatedly but can create a single function that executes the specific commands. The definition of such functions makes the code easier to read and helps to keep the code as short as possible. It is important to note that functions must always be defined logically `before` the first call since a script is also processed from top to bottom. Therefore the definition of a function is always `at the beginning` of the script. There are two methods to define the functions:

#### Method 1 - Functions

```bash
function name {
	<commands>
}
```

#### Method 2 - Functions

```bash
name() {
	<commands>
}
```

We can choose the method to define a function that is most comfortable for us. In our `CIDR.sh` script, we used the first method because it is easier to read with the keyword "`function`."

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

The function is called only by calling the specified name of the function, as we have seen in the case statement.

#### Function Execution - CIDR.sh

```bash
<SNIP>
case $opt in
	"1") network_range ;;
	"2") ping_host ;;
	"3") network_range && ping_host ;;
	"*") exit 0 ;;
esac
```

## Parameter Passing

Such functions should be designed so that they can be used with a fixed structure of the values or at least only with a fixed format. Like we have already seen in our `CIDR.sh` script, we used the format of an IP address for the function "`network_range`". The parameters are optional, and therefore we can call the function without parameters. In principle, the same applies to the passed parameters as to parameters passed to a shell script. These are `$1` - `$9` (`${n}`), or `$variable` as we have already seen. Each function has its own set of parameters. So they do not collide with those of other functions or the parameters of the shell script.

An important difference between bash scripts and other programming languages is that all defined variables are always processed `globally` unless otherwise declared by "[local](https://www.tldp.org/LDP/abs/html/localvar.html)." This means that the first time we have defined a variable in a function, we will call it in our main script (outside the function). Passing the parameters to the functions is done the same way as we passed the arguments to our script and looks like this:

#### PrintPars.sh

```bash
#!/bin/bash

function print_pars {
	echo $1 $2 $3
}

one="First parameter"
two="Second parameter"
three="Third parameter"

print_pars "$one" "$two" "$three"
```

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ ./PrintPars.sh

First parameter Second parameter Third parameter
```

## Return Values

When we start a new process, each `child process` (for example, a `function` in the executed script) returns a `return code` to the `parent process` (`bash shell` through which we executed the script) at its termination, informing it of the status of the execution. This information is used to determine whether the process ran successfully or whether specific errors occurred. Based on this information, the `parent process` can decide on further program flow.

| **Return Code** | **Description**                |
| --------------- | ------------------------------ |
| `1`             | General errors                 |
| `2`             | Misuse of shell builtins       |
| `126`           | Command invoked cannot execute |
| `127`           | Command not found              |
| `128`           | Invalid argument to exit       |
| `128+n`         | Fatal error signal "`n`"       |
| `130`           | Script terminated by Control-C |
| `255\*`         | Exit status out of range       |

To get the value of a function back, we can use several methods like `return`, `echo`, or a `variable`. In the next example, we will see how to use "`$?`" to read the "`return code`," how to pass the arguments to the function and how to assign the result to a variable.

#### Return.sh

```bash
#!/bin/bash

function given_args {

        if [ $# -lt 1 ]
        then
                echo -e "Number of arguments: $#"
                return 1
        else
                echo -e "Number of arguments: $#"
                return 0
        fi
}

# No arguments given
given_args
echo -e "Function status code: $?\n"

# One argument given
given_args "argument"
echo -e "Function status code: $?\n"

# Pass the results of the funtion into a variable
content=$(given_args "argument")

echo -e "Content of the variable: \n\t$content"
```

#### Return.sh - Execution

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ ./Return.sh

Number of arguments: 0
Function status code: 1

Number of arguments: 1
Function status code: 0

Content of the variable:
    Number of arguments: 1
```