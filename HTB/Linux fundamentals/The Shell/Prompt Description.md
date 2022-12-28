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