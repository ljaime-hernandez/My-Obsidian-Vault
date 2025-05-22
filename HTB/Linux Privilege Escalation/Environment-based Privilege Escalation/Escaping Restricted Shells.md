A restricted shell is a type of shell that limits the user's ability to execute commands. In a restricted shell, the user is only allowed to execute a specific set of commands or only allowed to execute commands in specific directories. Restricted shells are often used to provide a safe environment for users who may accidentally or intentionally damage the system or provide a way for users to access only certain system features. Some common examples of restricted shells include the `rbash` shell in Linux and the "Restricted-access Shell" in Windows.

#### RBASH

[Restricted Bourne shell](https://www.gnu.org/software/bash/manual/html_node/The-Restricted-Shell.html) (`rbash`) is a restricted version of the Bourne shell, a standard command-line interpreter in Linux which limits the user's ability to use certain features of the Bourne shell, such as changing directories, setting or modifying environment variables, and executing commands in other directories. It is often used to provide a safe and controlled environment for users who may accidentally or intentionally damage the system.

#### RKSH

[Restricted Korn shell](https://www.ibm.com/docs/en/aix/7.2?topic=r-rksh-command) (`rksh`) is a restricted version of the Korn shell, another standard command-line interpreter. The `rksh` shell limits the user's ability to use certain features of the Korn shell, such as executing commands in other directories, creating or modifying shell functions, and modifying the shell environment.

#### RZSH

[Restricted Z shell](https://manpages.debian.org/experimental/zsh/rzsh.1.en.html) (`rzsh`) is a restricted version of the Z shell and is the most powerful and flexible command-line interpreter. The `rzsh` shell limits the user's ability to use certain features of the Z shell, such as running shell scripts, defining aliases, and modifying the shell environment.

For example, administrators often use restricted shells in enterprise networks to provide a safe and controlled environment for users who may accidentally or intentionally damage the system. By limiting the user's ability to execute specific commands or access certain directories, administrators can ensure that users cannot perform actions that could harm the system or compromise the network's security. Additionally, restricted shells can give users access to only certain system features, allowing administrators to control which resources and functions are available to each user.

Imagine a company with a network of Linux servers hosting critical business applications and services. Many users, including employees, contractors, and external partners, access the network. To protect the security and integrity of the network, the organization's IT team decided to implement restricted shells for all users.

To do this, the IT team sets up several `rbash`, `rksh`, and `rzsh` shells on the network and assigns each user to a specific shell. For example, external partners who need to access only certain network features, such as email and file sharing, are assigned to `rbash` shells, which limits their ability to execute specific commands and access certain directories. Contractors who need to access more advanced network features, such as database servers and web servers, are assigned to `rksh` shells, which provide them with more flexibility but still limit their abilities. Finally, employees who need to access the network for specific purposes, such as to run specific applications or scripts, are assigned to `rzsh` shells, which provide them with the most flexibility but still limit their ability to execute specific commands and access certain directories.

Several methods can be used to escape from a restricted shell. Some of these methods involve exploiting vulnerabilities in the shell itself, while others involve using creative techniques to bypass the restrictions imposed by the shell. Here are a few examples of methods that can be used to escape from a restricted shell.

## Escaping

In some cases, it may be possible to escape from a restricted shell by injecting commands into the command line or other inputs the shell accepts. For example, suppose the shell allows users to execute commands by passing them as arguments to a built-in command. In that case, it may be possible to escape from the shell by injecting additional commands into the argument.

#### Command injection

Imagine that we are in a restricted shell that allows us to execute commands by passing them as arguments to the `ls` command. Unfortunately, the shell only allows us to execute the `ls` command with a specific set of arguments, such as `ls -l` or `ls -a`, but it does not allow us to execute any other commands. In this situation, we can use command injection to escape from the shell by injecting additional commands into the argument of the `ls` command.

For example, we could use the following command to inject a `pwd` command into the argument of the `ls` command:

  Escaping Restricted Shells

```shell-session
KaoruRyusaki@htb[/htb]$ ls -l `pwd` 
```

This command would cause the `ls` command to be executed with the argument `-l`, followed by the output of the `pwd` command. Since the `pwd` command is not restricted by the shell, this would allow us to execute the `pwd` command and see the current working directory, even though the shell does not allow us to execute the `pwd` command directly.

#### Command Substitution

Another method for escaping from a restricted shell is to use command substitution. This involves using the shell's command substitution syntax to execute a command. For example, imagine the shell allows users to execute commands by enclosing them in backticks ( \` ). In that case, it may be possible to escape from the shell by executing a command in a backtick substitution that is not restricted by the shell.

#### Command Chaining

In some cases, it may be possible to escape from a restricted shell by using command chaining. We would need to use multiple commands in a single command line, separated by a shell metacharacter, such as a semicolon (`;`) or a vertical bar (`|`), to execute a command. For example, if the shell allows users to execute commands separated by semicolons, it may be possible to escape from the shell by using a semicolon to separate two commands, one of which is not restricted by the shell.

#### Environment Variables

For escaping from a restricted shell to use environment variables involves modifying or creating environment variables that the shell uses to execute commands that are not restricted by the shell. For example, if the shell uses an environment variable to specify the directory in which commands are executed, it may be possible to escape from the shell by modifying the value of the environment variable to specify a different directory.

#### Shell Functions

In some cases, it may be possible to escape from a restricted shell by using shell functions. For this we can define and call shell functions that execute commands not restricted by the shell. Let us say, the shell allows users to define and call shell functions, it may be possible to escape from the shell by defining a shell function that executes a command.



#### Questions

Use different approaches to escape the restricted shell and read the flag.txt file. Submit the contents as the answer.

* Download the VPN file from the HTB Academy webpage
* Open your VM and go to your Downloads page from the terminal
* Run the command " openvpn academy-regular.ovpn " (if it doesnt work try running the same command with sudo)
* Use the user and password given by HTB and then SSH to the target system spawned for you by running this command "ssh htb-user@\<ip given>\"
* Explore common commands and see which ones are not restricted, as pwd or echo
* Research how to open the content of a file with echo
* Run command 'echo "$(<flag.txt )" '
* HTB{35c4p3_7h3_r3stricted_5h311}
