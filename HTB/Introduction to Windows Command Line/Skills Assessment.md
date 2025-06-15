During a penetration test with our team, we were instructed to collect some information for a non-critical Windows host. Our team has recently gained access to this host, which contains many users. For the team to focus on more complex tasks, we have been asked to take a closer look at this host. The main focus is on finding user names and passwords. This should enable us to examine the system in such a way that we can find out and document all user passwords.

Each question has a corresponding `user` with whom you will need to authenticate to complete the questions. In each challenge, you may be asked to perform specific actions, use specific executables, or find information on the host to get the flag for that question.

In most instances, the flag for the previous user must be used as the SSH password for the following user (i.e., the flag for user2 is the password for user3 to SSH in, and so on).





#### Questions

The flag will print in the banner upon successful login on the host via SSH.

* D0wn_the_rabbit_H0!3

Access the host as user1 and read the contents of the file "flag.txt" located in the users Desktop.

* Run Command " chdir Desktop"
* Run command " type flag.txt "
* Nice and Easy!

If you search and find the name of this host, you will find the flag for user2.

* Run Command " hostname "
* ACADEMY-ICL11

How many hidden files exist on user3's Desktop?

* Run Command " chdir Desktop "
* Run command " dir /A:H "
* 101 files are counted at the end of the output


User4 has a lot of files and folders in their Documents folder. The flag can be found within one of them.

* Run command " tree /F ", youll notice theres flags in every folder and theres hundreds of them. If you try opening a couple of them you will realize they generaly have no information inside, so we are going to look for a flag that has more then one byte
* Run command " forfiles /P "C:\Users\user4\Documents" /S /C "cmd /c if @fsize GTR 1 echo @path ". The only file that will pop up is located in " C:\Users\user4\Documents\3\4\flag.txt " 
* Run command " type C:\Users\user4\Documents\3\4\flag.txt "
* Digging in The nest


How many users exist on this host? (Excluding the DefaultAccount and WDAGUtility)

* Run command " where /R C:\Users \*user* " 
* the output will show some folders where you can count the amount of " user pictures ", do not count the default user
* 14


For this level, you need to find the Registered Owner of the host. The Owner name is the flag.

* Run command " system info ", the registered owner will show in one of the text lines
* htb-student


For this level, you must successfully authenticate to the Domain Controller host at 172.16.5.155 via SSH after first authenticating to the target host. This host seems to have several PowerShell modules loaded, and this user's flag is hidden in one of them.

* Run command " where /R C:\Users \*.psm1 ", several files with the name " Flag-Finder.psm1 " will pop up
* Run command " type "C:\Users\user7\My Documents
\WindowsPowerShell\Modules\Flag-Finder\Flag-Finder.psm1" " (random file i chose from the list) and read through the file, the flag will be there
* Modules_make_pwsh_run!


This flag is the GivenName of a domain user with the Surname "Flag".

* Run command " net user /domain ", theres a curious user called " RFlag "
* Run command " net user RFlag /domain " to review its information, youll find the given name there
* Rick


Use the tasklist command to print running processes and then sort them in reverse order by name. The name of the process that begins with "vm" is the flag for this user.

* Run command " tasklist /svc | sort /R ", take the name of the first service which name starts with " vm "
* vmtoolsd.exe


What user account on the Domain Controller has many Event ID (4625) logon failures generated in rapid succession, which is indicative of a password brute forcing attack? The flag is the name of the user account.

* justalocaladmin
