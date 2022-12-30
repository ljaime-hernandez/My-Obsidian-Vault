One of the best ways to learn something new is to experiment with it. Here we cover the sections on how to navigate through Linux, create, move, edit and delete files and folders, find them on the operating system, different types of redirects, and what file descriptors are. Then we will find some shortcuts that will make our work with the shell easier. It is recommended to experiment on our locally hosted VM. Make sure we have created a snapshot for our VM in case our system gets unexpectedly damaged.

Let us start with the navigation. Before we move through the system, we have to find out in which directory we are. We can find out where we are with the command `pwd`.

```shell-session
cry0l1t3@htb[~]$ pwd

/home/cry0l1t3
```

Only the `ls` command is needed for navigation. It has many additional options that can complement the display of the content in the current folder.

```shell-session
cry0l1t3@htb[~]$ ls

Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```

```shell-session
cry0l1t3@htb[~]$ ls -l

total 32
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:37 Desktop
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:34 Documents
drwxr-xr-x 3 cry0l1t3 cry0l1t3 4096 Nov 15 03:26 Downloads
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:34 Music
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:34 Pictures
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:34 Public
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:34 Templates
drwxr-xr-x 2 cry0l1t3 cry0l1t3 4096 Nov 13 17:34 Videos
```

However, we will not see everything that is in this folder. To list the contents of a directory, we do not necessarily need to navigate there first. We can also use "`ls`" to specify the path where we want to know the contents.

```shell-session
cry0l1t3@htb[~]$ ls -l /var/

total 52
drwxr-xr-x  2 root root     4096 Mai 15 18:54 backups
drwxr-xr-x 18 root root     4096 Nov 15 16:55 cache
drwxrwsrwt  2 root whoopsie 4096 Jul 25  2018 crash
drwxr-xr-x 66 root root     4096 Mai 15 03:08 lib
drwxrwsr-x  2 root staff    4096 Nov 24  2018 local 
<SNIP>
```

We can do the same thing if we want to navigate to the directory. To move through the directories, we use the command `cd`. Let us change to the `/dev/shm` directory. Of course, we can go to the `/dev` directory first and then `/shm`. Nevertheless, we can also directly enter the full path and jump directly there.

```shell-session
cry0l1t3@htb[~]$ cd /dev/shm

cry0l1t3@htb[/dev/shm]$
```

Since we were in the home directory before, we can quickly jump back to the directory we were last in.

```shell-session
cry0l1t3@htb[/dev/shm]$ cd -

cry0l1t3@htb[~]$ 
```

The shell also offers us the auto-complete function, which makes navigation easier. If we now type `cd /dev/s` and then press `[TAB] twice`, we will get all entries starting with the letter "`s`" in the directory of `/dev/`.

```shell-session
cry0l1t3@htb[~]$ cd /dev/s [TAB 2x]

shm/ snd/
```

If we add the letter "`h`" to the letter "`s`," the shell will complete the input since otherwise there will be no folders in this directory beginning with the letters "`sh`". If we now display all contents of the directory, we will only see the following contents.

```shell-session
cry0l1t3@htb[/dev/shm]$ ls -la

total 0
drwxrwxrwt  2 root root   40 Mai 15 18:31 .
drwxr-xr-x 17 root root 4000 Mai 14 20:45 ..
```

The first entry with a single dot (`.`) indicates the current directory we are currently in. The second entry with two dots (`..`) represents the parent directory `/dev`. This means that we can jump to the parent directory with the following command.

```shell-session
cry0l1t3@htb[/dev/shm]$ cd ..

cry0l1t3@htb[/dev]$
```

Since our shell is filled with some records, we can clean the shell with the command `clear`. However, let us return to the directory `/dev/shm` before.

```shell-session
cry0l1t3@htb[/dev]$ cd shm && clear
```

