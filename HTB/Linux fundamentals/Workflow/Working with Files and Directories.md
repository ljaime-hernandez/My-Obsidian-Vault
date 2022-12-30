## Create, Move, and Copy

Next, let us work with files and directories and learn how to create, rename, move, copy, and delete. First, let us create an empty file and a directory. We can use `touch` to create an empty file and `mkdir` to create a directory.

The syntax for this is the following:

Syntax - touch

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ touch <name>
```

Syntax - mkdir

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ mkdir <name>
```

In this example, we name the file `info.txt` and the directory `Storage`. To create these, we follow the commands and their syntax shown above.

Create an Empty File

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ touch info.txt
```

Create a Directory

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ mkdir Storage
```

We may want to have specific directories in the directory, and it would be very time-consuming to create this command for every single directory. The command `mkdir` has an option marked `-p` to add parent directories.

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ mkdir -p Storage/local/user/documents
```

We can look at the whole structure after creating the parent directories with the tool `tree`.

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ tree .

.
├── info.txt
└── Storage
    └── local
        └── user
            └── documents

4 directories, 1 file
```

We can also create files directly in the directories by specifying the path where the file should be stored. The trick is to use the single dot (`.`) to tell the system that we want to start from the current directory. So the command for creating another empty file looks like this:

Create userinfo.txt

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ touch ./Storage/local/user/userinfo.txt
```

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ tree .

.
├── info.txt
└── Storage
    └── local
        └── user
            ├── documents
            └── userinfo.txt

4 directories, 2 files
```

With the command `mv`, we can move and also rename files and directories. The syntax for this looks like this:

Syntax - mv

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ mv <file/directory> <renamed file/directory>
```

First, let us rename the file `info.txt` to `information.txt` and then move it to the directory `Storage`.

Rename File

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ mv info.txt information.txt
```

Now let us create a file named `readme.txt` in the current directory and then copy the files `information.txt` and `readme.txt` into the `Storage/` directory.

Create readme.txt

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ touch readme.txt 
```

Move Files to Specific Directory

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ mv information.txt readme.txt Storage/
```

  Move Files to Specific Directory

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ tree .

.
└── Storage
    ├── information.txt
    ├── local
    │   └── user
    │       ├── documents
    │       └── userinfo.txt
    └── readme.txt

4 directories, 3 files
```

Let us assume we want to have the `readme.txt` in the `local/` directory. Then we can copy them there with the paths specified.

Copy readme.txt

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ cp Storage/readme.txt Storage/local/
```

Now we can check if the file is thereby using the tool `tree` again.

```shell-session
Luis Miguel Jaime Hernandez@htb[/htb]$ tree .

.
└── Storage
    ├── information.txt
    ├── local
    │   ├── readme.txt
    │   └── user
    │       ├── documents
    │       └── userinfo.txt
    └── readme.txt

4 directories, 4 files
```

