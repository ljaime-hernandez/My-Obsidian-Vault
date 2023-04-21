The bash prompt is easy to understand and, by default, includes information such as the user, hostname, and current working directory. The format can look something like this:

```shell-session
<username>@<hostname><current working directory>$
```

The home directory for a user is marked with a tilde <`~`> and is the default folder when we log in.

```shell-session
<username>@<hostname>[~]$
```

The dollar sign, in this case, stands for a user. As soon as we log in as `root`, the character changes to a `hash` <`#`> and looks like this:

```shell-session
root@htb[/htb]#
```

We see here the same as when we work on the Windows GUI. We are logged in as a user on a computer with a specific name, and we know which directory we are in when we navigate through our system. Bash prompt can also be customized and changed to our own needs. The adjustment of the bash prompt is outside the scope for this module. However, we can look at the [bashrcgenerator](http://bashrcgenerator.com/) and [powerline](https://github.com/powerline/powerline), which gives us the possibility to adapt our prompt to our needs.

For example, when we upload and run a shell on the target system, we may not see the username, hostname, and current working directory. This may be due to the PS1 variable in the environment not being set correctly. In this case, we would see the following prompts:

### Unprivileged - User Shell Prompt

```shell-session
$
```

### Privileged - Root Shell Prompt

```shell-session
#
```

In addition to providing basic information like the current user and working directory, we can customize to display other information in the prompt, such as the date and time, IP address, date, time, the exit status of the last command, and more. This is especially useful for us during our penetration tests because we can use various tools and possibilities like `script` or the `.bash_history` to filter and print all the commands we used and sort them by date and time. For example, the prompt could be set to display the full path of the current working directory instead of just the current directory name, which can also include the target’s IP address if we work organized.

The prompt can be customized using special characters and variables in the shell’s configuration file (`.bashrc` for the Bash shell). For example, we can use: the `\u` character to represent the current username, `\h` for the hostname, and `\w` for the current working directory.

| **Special Character** | **Description** | 
| --------------------- | --------------- |
| `\d` | Date (Mon Feb 6)
| `\D{%Y-%m-%d}` | Date (YYYY-MM-DD)
| `\H` | Full hostname
| `\j` | Number of jobs managed by the shell
| `\n` | Newline
| `\r` | Carriage return
| `\s` | Name of the shell
| `\t` | Current time 24-hour (HH:MM:SS)
| `\T` | Current time 12-hour (HH:MM:SS)
| `\@` | Current time
| `\u` | Current username
| `\w` | Full path of the current working directory |

Customizing the prompt can be a useful way to make your terminal experience more personalized and efficient. It can also be a helpful tool for troubleshooting and problem-solving, as it can provide important information about the system’s state at any given time.

In addition to customizing the prompt, we can customize their terminal environment with different color schemes, fonts, and other settings to make their work environment more visually appealing and easier to use.

However, we see the same as when working on the Windows GUI here. We are logged in as a user on a computer with a specific name, and we know which directory we are in when we navigate through our system. Bash prompt can also be customized and changed to our own needs. The adjustment of the bash prompt is outside the scope of this module. However, we can look at the [bashrcgenerator](http://bashrcgenerator.com/) and [powerline](https://github.com/powerline/powerline), which gives us the possibility to adapt our prompt to our needs.