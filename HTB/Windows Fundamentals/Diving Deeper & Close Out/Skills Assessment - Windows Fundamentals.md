Inlanefreight recently had an incident where a disgruntled employee in marketing accessed an internally hosted HR share and deleted several confidential files & folders. Thankfully, the IT team had good backups and restored all of the deleted data. There are now concerns that this disgruntled employee was able to access the HR share in the first place. After performing a security assessment, you have found that the IT team may not fully understand how permissions work in Windows. You are conducting training and a demonstration to show the department good security practices with file sharing in a Windows environment as well as viewing services from the command line to check for any potential tampering.

Note: It is important that each step is completed in the order they are presented. Starting with step 1 and working your way through step 8, including all associated specifications with each step. Please know that each step is designed to give you the opportunity to apply the skills & concepts taught throughout this module. Take your time, have fun and feel free to reach out if you get stuck.

In this demonstration, you are:

1. Creating a shared folder called Company Data
2. Creating a subfolder called HR inside of the Company Data folder
3. Creating a user called Jim
	-   `Uncheck: User must change password at logon`
4. Creating a security group called HR
5. Adding Jim to the HR security group
6. Adding the HR security group to the shared Company Data folder and NTFS permissions list
	-   `Remove the default group that is present`
	-   `Share Permissions: Allow Change & Read`
	-   `Disable Inheritance before issuing specific NTFS permissions`
	-   `NTFS permissions: Modify, Read & Execute, List folder contents, Read, Write`
7. Adding the HR security group to the NTFS permissions list of the HR subfolder
	-   `Remove the default group that is present`
	-   `Disable Inheritance before issuing specific NTFS permissions`
	-   `NTFS permissions: Modify, Read & Execute, List folder contents, Read, and Write`
8. Using PowerShell to list details about a service

What is the name of the group that is present in the Company Data Share Permissions ACL by default?

`Everyone`
check after creating a folder normally

What is the name of the tab that allows you to configure NTFS permissions?

`Security`
after allowing shared permissions on a folde

What is the name of the service associated with Windows Update?

`wuauserv`
check on task manager services

List the SID associated with the user account Jim you created.

`S-1-5-21-2614195641-1726409526-3792725429-1006`
wmic
useraccount

List the SID associated with the HR security group you created.

`S-1-5-21-2614195641-1726409526-3792725429-1007`
Get-WMIObject win32_group -filter "name='HR'"|select Name,sid