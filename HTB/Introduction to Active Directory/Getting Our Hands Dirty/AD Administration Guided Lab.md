Let us consider the following case:

![image](https://academy.hackthebox.com/storage/modules/74/helping-out.png)

In this section, we will serve as domain administrators to Inlanefreight for a day. We have been tasked to help the IT department close some work orders, so we will be performing actions such as adding and removing users and groups, managing group policy, and more. Successful completion of the tasks can lead to us gaining a promotion to the Tier II IT team from the helpdesk.

## Connection Instructions

For this lab, you will have access to a domain-joined Windows server from which you can perform any actions needed to complete the lab. The environment will require you to RDP from Pwnbox or your own VM over VPN to the Windows server. Follow the steps below to utilize `RDP` and connect to the lab's Windows host.

-   Click below in the `Questions` section to spawn the target host and obtain an IP address. The image below shows where to spawn the target and acquire a VPN key for the lab if needed.
    -   IP ==
    -   Username == `htb-student_adm`
    -   Password == `Academy_student_DA!`
-   We will use `xfreerdp` to connect with the target.
-   Open a terminal in pwnbox or from your lab vm over vpn and enter the following command:
    -   xfreerdp /v: /u:`htb-student_adm` /p:`Academy_student_DA!`

Once connected, open an MMC console, PowerShell, or the ADDS tools to begin.

## Tasks:

Attempt to complete the challenges on your own. If you get stuck, the `Solutions` dropdown below each task can help you. [This](https://docs.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps) reference on the Active Directory PowerShell module will be extremely helpful. As an introductory course on AD, we do not expect you to know everything about the topic and how to administer it. The Solutions below each task offer a step-by-step of how to complete the task. This section is provided to give you a taste of the daily tasks that AD administrators perform. Instead of providing the information to you in a static format, we have opted to provide it in a more hands-on manner.

#### Task 1: Manage Users

Our first task of the day includes adding a few new-hire users into AD. We are just going to create them under the `"inlanefreight.local"` scope, drilling down into the `"Corp > Employees > HQ-NYC > IT "` folder structure for now. Once we create our other groups, we will move them into the new folders. You can utilize the Active Directory PowerShell module (New-ADUser), the Active Directory Users and Computers snap-in, or MMC to perform these actions.

#### Users to Add:

| **User**            |
| ------------------- |
| `Andromeda Cepheus` |
| `Orion Starchaser`  |
| `Artemis Callisto`  |

Each user should have the following attributes set, along with their name:

| **Attribute**                                                                            |
| ---------------------------------------------------------------------------------------- |
| `full name`                                                                              |
| `email (first-initial.lastname@inlanefreight.local) ( ex. j.smith@inlanefreight.local )` |
| `display name`                                                                           |
| `User must change password at next logon`                                                |

Once we have added our new hires, take a quick second and remove a few old user accounts found in an audit that are no longer required.

#### Users to Remove

| **User**        |
| --------------- |
| `Mike O'Hare`   |
| `Paul Valencia` |

Lastly `Adam Masters` has submitted a trouble ticket over the phone saying his account is locked because he typed his password wrong too many times. The helpdesk has verified his identity and that his Cyber awareness training is up to date. The ticket requests that you unlock his user account and force him to change his password at the next login.

![image](https://academy.hackthebox.com/storage/modules/74/troubleticket.png)

# `Solution: Task 1`

Open PowerShell as an administrator. To ADD a user into Active Directory, we First need to load the module with the "Import-Module -Name ActiveDirectory" cmdlet. The AD module can be installed via the RSAT feature pack, but for now, it's already installed on the host used in this lab.

#### PowerShell Terminal Output for Adding a User

```powershell-session
PS C:\htb> New-ADUser -Name "Orion Starchaser" -Accountpassword (ConvertTo-SecureString -AsPlainText (Read-Host "Enter a secure password") -Force ) -Enabled $true -OtherAttributes @{'title'="Analyst";'mail'="o.starchaser@inlanefreight.local"}
```

After you hit enter, a prompt will appear, enter a secure password for the user.

#### Adding a User from the MMC Snap-in

Before adding a user from the GUI we need to open the Active Directory Users and Computers (ADUC) MMC tool. As a standard user, we may have access to view the ADUC objects, but we will not be able to modify or add. We need to log in as our administrator account (credentials above) to complete these actions. Once logged in, open the ADUC snap-in by performing the following actions:

-   From the Server Manager window, select Tools > then ADUC
-   Expand the scope "inlanefreight.local" and drill down to "Corp > Employees > HQ-NYC > IT ". This is where we will be creating our new users, OU's, and Groups.

#### Adding an AD User via the GUI

To add an AD user via the GUI we first need to open Active Directory Users and Computers via the Start Menu folder Administrative Tools.

1. Right click on "IT", Select "New" > "User".

![](https://academy.hackthebox.com/storage/modules/74/add-user1.png)

2. Add the users First and Last name, set the "User Logon Name:" as `acepheus` and then hit Next.

![](https://academy.hackthebox.com/storage/modules/74/add-user2.png)

3. Set a password of `NewP@ssw0rd123!`and check the box for "User must change password at next login".

![](https://academy.hackthebox.com/storage/modules/74/add-user3.png)

4. If all attributes look correct, select "Finish" in the last window

![](https://academy.hackthebox.com/storage/modules/74/add-user4.png)
  
5. Our new User exists now in the OU.

![](https://academy.hackthebox.com/storage/modules/74/add-user5.png)

#### Add A User

We will add the new user `Andromeda Cepheus` to our domain. We can do so by:

-   Right-click on "IT" > Select "New" > "User". A popup window will appear with a field for you to fill in.
-   Add the user's First and Last name, set the "User Logon Name:" as `acepheus`, and then hit Next.
-   Now supply the new user with a password of `NewP@ssw0rd123!`, confirm the password again, and check the box for " User must change password at next login", then hit next. Select "Finish" in the last window if all attributes look correct.

To `REMOVE` a user account from Active Directory, we can:

#### PowerShell to Remove a User

  PowerShell to Remove a User

```powershell-session
PS C:\htb> Remove-ADUser -Identity pvalencia
```

The `Remove-ADUser` cmdlet above targets the user by its user logon name. Ensure you are targeting the right user before executing it. If we are unsure of the value needed, we can use the `Get-ADUser` command to validate first.

#### Remove a User from the MMC Snap-in

Now we will remove a user `Paul Valencia` from our domain. We can do so by:

-   The most straightforward method from the ADUC snap-in will be to use the `find` functionality. Inlanefreight has many users across several OU's. To use find:
    -   Right-click on `Employees` and select "find".
    -   Type in the username you wish to search for, in this case, "Paul Valencia" and hit "Find Now." If a user has that name, the search results will appear lower in the find window.
-   Now, right-click on the user and select delete. A popup window will appear to confirm the deletion of the user. Hit yes.
-   To validate the user is deleted, you can use the `Find` feature again to search for the user.

#### Deleting a User via the GUI

To delete a user via the GUI, we will use the ADUC snap-in just like when we added a user to the domain above.

1. Right click on the "Employees OU" and select "find".

![](https://academy.hackthebox.com/storage/modules/74/del-user1.png)

2. Type in the username you wish to search for, in this case "Paul Valencia" and hit "Find Now".

![](https://academy.hackthebox.com/storage/modules/74/del-user2.png)

3. Right click on the user and select delete.

![](https://academy.hackthebox.com/storage/modules/74/del-user3.png)

4. Confirm the deletion in the pop-up window. Find can be utilized again to determine if the user is gone.

![](https://academy.hackthebox.com/storage/modules/74/del-user4.png)

Now we need to help `Adam Masters` out and unlock his account again.

To `UNLOCK` a user account we can:

#### PowerShell To Unlock a User

```powershell-session
PS C:\htb> Unlock-ADAccount -Identity amasters 
```

We also need to set a new password for the user and force them to change the password at the next logon. We will do this with the `SetADAccountPassword` and `Set-ADUser` cmdlets.

#### Reset User Password (Set-ADAccountPassword)

```powershell-session
PS C:\htb> Set-ADAccountPassword -Identity 'amasters' -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "NewP@ssw0rdReset!" -Force)
```

#### Force Password Change (Set-ADUser)

```powershell-session
PS C:\htb> Set-ADUser -Identity amasters -ChangePasswordAtLogon $true
```

#### Unlock from Snap-in

Unlocking this user account will take several steps. The first is to unlock the account, then we set it so that the user must change his password at the next login, and then we reset his password to a temporary one so that he can log in and reset it himself. We can do so by:

-   right-click on the user and select `Reset Password`.
-   In the next window, type in the temporary password, confirm it, and check the boxes for "User must change password at next logon" and "Unlock the user's account."
-   Once done, hit OK to apply changes. If no error occurs, you will get a prompt informing you that the user's password was changed.

#### Unlock Users Account From GUI

To unlock Adam Masters' account, we will use the ADUC snap-in just like when we added a user to the domain above.

1. Right click on Adam Master's account and select "Reset Password".

![](https://academy.hackthebox.com/storage/modules/74/unlock-1.png)

2. Set a new temporary password and select the "Unlock" and "User must change password" dialog boxes.

![](https://academy.hackthebox.com/storage/modules/74/unlock-2.png)

## Task 2: Manage Groups and Other Organizational Units

Next up for us is to create a new Security Group called `Analysts` and then add our new hires into the group. This group should also be nested in an OU named the same under the `IT` hive. The `New-ADOrganizationalUnit` PowerShell command should enable you to quickly add a new security group. We can also utilize the AD Users and Computers snap-in like in Task-1 to complete this task.

# `Solution: Task 2`

#### Create a New AD OU and Security Group from PowerShell

To create a new OU and Group, we can perform the following actions:

```powershell-session
PS C:\htb> New-ADOrganizationalUnit -Name "Security Analysts" -Path "OU=IT,OU=HQ-NYC,OU=Employees,OU=CORP,DC=INLANEFREIGHT,DC=LOCAL""
```

First, we created the new OU to hold our Analysts and their resources. Next, we need to create a security group for these users.

```powershell-session
PS C:\htb> New-ADGroup -Name "Security Analysts" -SamAccountName analysts -GroupCategory Security -GroupScope Global -DisplayName "Security Analysts" -Path "OU=Security Analysts,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL" -Description "Members of this group are Security Analysts under the IT OU"
```

#### From MMC Snap-in

This will be a quick two-step process for us. We first need to create a new OU to host our Security Analysts. To do so, we will :

-   navigate to the "Corp > Employees > HQ-NYC > IT "OU. We are going to build out a new container within `IT`.
-   Right-click on `IT` and select "New > Organizational Unit". A new window should appear.
    -   input the name `Security Analysts` into the Name field and leave the default option set for the Protect checkbox. Hit OK, and the OU should be created.

#### Create A New OU Under I.T.

Our new OU "Security Analysts" should exist in the IT hive.

1. From within the IT OU, Right click and select "New" > "Organizational Unit"

![](https://academy.hackthebox.com/storage/modules/74/new-ou1.png)

2. Type in the name for the OU, "Security Analysts" in this instance. Hit OK when done.

![](https://academy.hackthebox.com/storage/modules/74/new-ou2.png)

Now that we have our OU, let's create the `Security Group` for our Analysts.  
Right-click on our new OU `Security Analysts` and select "New > Group" and a popup window should appear.

-   Input the name of the group `Security Analysts`
-   Select the Group scope `Domain local`
-   ensure group type says `Security` not "Distribution".
-   Once you check the options, hit OK.

#### Creating A Security Group

Our Security Group will go in the OU we just created.

1. right click on our new OU `Security Analysts` and select "New > Group". A popup window should appear.

![](https://academy.hackthebox.com/storage/modules/74/new-group1.png)

2. Enter a Name, scope, and type, then hit OK.

![](https://academy.hackthebox.com/storage/modules/74/new-group2.png)

When done, a new Security Group should exist in our OU. We need to move our new users into the OU and add them to the security group. Keep in mind the purpose of this is to logically organize our AD objects for easy location and administration. Utilizing the security groups, we can quickly assign permissions and resources to specific users instead of managing each user individually.

To ADD a user to a `group`, we can:

#### Add User to Group via PowerShell

```powershell-session
PS C:\htb> Add-ADGroupMember -Identity analysts -Members ACepheus,OStarchaser,ACallisto
```

Here we use the `SAMAccountName` of the users to add them to the Analysts group via the `Add-ADGroup Member` Cmdlet. Ensure your list is comma separated without spaces between each.

#### From MMC Snap-in

To add the users to the security group, we can:

-   Find the user you wish to add
-   Right-click on the user and select "Add to a group". A new window will appear for you to specify the group name.
-   type in part or all of the group you wish to add the user to. In this case, we are adding Andromeda to the Security Analysts group. If our query matches one or more groups, another dialog box will appear, providing us with a list of groups to choose from. Pick the group you need and hit "OK".
-   The choice you selected will now be highlighted in the previous window. More than one group can be selected at a time if necessary. Once done, hit "OK."
-   If no issues arise, you will get a new popup informing you that the operation is completed. To validate, we can view the group or user properties.

#### Add A User To A Security Group

In this example we are adding Andromeda to the Security Analysts group, then moving her into the correct OU.

1. Right click on the user and select "Add to a group"

![](https://academy.hackthebox.com/storage/modules/74/user-group1.png)

2. Enter a full or partial group name in the search box and hit "Check Names".

![](https://academy.hackthebox.com/storage/modules/74/user-group2.png)

3. Once you have selected the correct group ( it will be underlined if correct ) hit OK.

![](https://academy.hackthebox.com/storage/modules/74/user-group3.png)


Thats `two` of our major tasks for the day done. Now let's move on to managing some Group Policy Objects.

## Task 3: Manage Group Policy Objects

Next, we have been asked to duplicate the group policy `Logon Banner`, rename it `Security Analysts Control`, and modify it to work for the new Analysts OU. We will need to make the following changes to the Policy Object:

-   we will be modifying the Password policy settings for users in this group and expressly allowing users to access PowerShell and CMD since their daily duties require it.
-   For computer settings, we need to ensure the Logon Banner is applied and that removable media is blocked from access.

Once done, make sure the Group Policy is applied to the `Security Analysts` OU. This will require the use of the Group Policy Management snap-in found under `Tools` in the Server Manager window. For more of a challenge, the `Copy-GPO` cmdlet in PowerShell can also be utilized.

# `Solution: Task 3`

To Duplicate a Group Policy Object we can use the `Copy-GPO` cmdlet or do it from the Group Policy Management Console.

#### Duplicate the Object via PowerShell

```powershell-session
PS C:\htb> Copy-GPO -SourceName "Logon Banner" -TargetName "Security Analysts Control"
```

The command above will take `Logon Banner` GPO and copy it to a new object named `Security Analyst Control`. This object will have all the old attributes of the Logon Banner GPO, but it will not be applied to anything until we link it.

#### Link the New GPO to an OU

```powershell-session
PS C:\htb> Set-GPLink -Name "Security Analysts Control" -Target "ou=Security Analysts,ou=IT,OU=HQ-NYC,OU=Employees,OU=Corp,dc=INLANEFREIGHT,dc=LOCAL" -LinkEnabled Yes
```

The command above will take the new GPO we created, link it to the OU `Security Analysts`, and enable it. For now, that's all we are going to do from PowerShell. We still need to make a few modifications to the policy, but we will perform these actions from Group Policy Management Console. Editing GPO preferences from PowerShell can be a bit daunting and way beyond the scope of this module.

#### Modify a GPO via GPMC

To modify our new policy object:

-   We need to open GPMC and expand the Group Policy Objects hive so we can see what GPOs exist.
    
-   Right-click on the policy object we wish to modify and select "Edit. The Group Policy Management Editor should pop up in a new window.
    
-   From here, we have several options to enable or disable.
    
-   We need to modify the removable media settings and ensure they are set to block any removable media from access. We will expressly allow security analysts to access PowerShell and CMD since their daily duties require it.
    
    -   location of removable media policy settings = `User Configuration > Policies > Administrative Templates > System > Removable Storage Access.`
    -   Location of Command Prompt settings = `User Configuration > Policies > Windows Settings > Administrative Templates > System.`
-   For `Computer settings`, we need to ensure the Logon Banner is applied and that the password policy settings for this group are strengthened.
    
    -   Location of Logon Banner settings = `Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options.`
    -   For reference, this setting should already be enabled since the GPO we copied was for a Logon Banner. We are validating the settings and ensuring it is enabled and applied.
    -   Location of Password Policy settings = `Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy.`

Let's get started.

#### User Configuration Group Policies

This slideshow will walk us through modifying group policies that affect Users directly. We will be modifying the policies affecting users access to the command prompt as well as their ability to use removeable media.

1. Right-click the GPO we wish to modify and select "Edit". This will bring up the Group Policy Configuration Editor window.

![](https://academy.hackthebox.com/storage/modules/74/edit-policy.png)

2. Drill down into the User Configuration policies to System > "Removable Storage Access". The policy we are going to edit is highlAdding an AD User via the GUIighted.

![](https://academy.hackthebox.com/storage/modules/74/storage-1.png)

3. Right click the setting and select "Edit".

![](https://academy.hackthebox.com/storage/modules/74/storage-2.png)

4. Check the radial button to enable the setting, hit "Apply" and then "OK".

![](https://academy.hackthebox.com/storage/modules/74/storage-3.png)

5. We can now see that our Policy setting is set to Enbled. Once we push policy to the domain, it will take effect.

![](https://academy.hackthebox.com/storage/modules/74/storage-4.png)

6. Next we are modifying the policy for Command Prompt access. Move to the System hive under User Configuration.

![](https://academy.hackthebox.com/storage/modules/74/cmd-1.png)

7. Right click and Edit the setting for "Prevent access to the command prompt".

![](https://academy.hackthebox.com/storage/modules/74/cmd-2.png)

8. We will select the radial button beside "Disabled" to explicitly allow the Security Analyst users to run command prompt and batch files as necessary for their role.

![](https://academy.hackthebox.com/storage/modules/74/cmd-3.png)

9. We can validate our policy settings are set in the view highlighted.

![](https://academy.hackthebox.com/storage/modules/74/cmd-4.png)

Now, let's modify the group policies affecting our `Computer` settings. We don't have to exit from the GPMC editor; we can just collapse the user configuration section and expand the Computer Configuration section.

#### Computer Configuration Group Policies

This slideshow will walk us through modifying group policies that affect computers in the group. We will be modifying the policies affecting the Logon Banner for the host, and setting a more restrictive password policy.

1. Move from the User Configuration hive into the Computer Configuration have. We will be validating the "Logon Banner" settings first. We validate the setting in "Interactive Logon Message Text" and "Interactive Logon Message Title".

![](https://academy.hackthebox.com/storage/modules/74/banner-1.png)

2. Right-click the setting and select Properties. Ensure the radial to define the policy setting is enabled and there is a Banner in the text box. If all appears good, hit OK.

![](https://academy.hackthebox.com/storage/modules/74/banner-2.png)

3. Change to the Message Title policy setting and validate the radial is selected, and a title of "Computer Access Policy" has been defined.

![](https://academy.hackthebox.com/storage/modules/74/banner-3.png)

4. Now, we will modify the settings for the Password Policies. Move into the Security Settings hive and click on "Password Policy" under the Account Policies dropdown. The policies on the right are what we will modify.

![](https://academy.hackthebox.com/storage/modules/74/password-1.png)

5. Starting with the "Minimum Password Length" setting. Right-click, select "Properties", and select the radial button to define the setting. Set the character count to ten. When done, apply and hit OK.

![](https://academy.hackthebox.com/storage/modules/74/password-2.png)

6. Now, we will enable "Password Complexity Requirements." Define the policy setting by clicking the radial button and then ensure "Enabled" is selected.

![](https://academy.hackthebox.com/storage/modules/74/password-3.png)

7. Next, we want to enforce password history for resetting the account password. Define the setting, and set the password history count to 5 previous passwords remembered. Hit Apply and OK.

![](https://academy.hackthebox.com/storage/modules/74/password-4.png)

8. Set the Minimum Password Age setting by defining the setting and applying a minimum age of 7 days. A new window will pop up telling us that the setting for "Maximum Password Age" will be set as well.

![](https://academy.hackthebox.com/storage/modules/74/password-5.png)

9. Validate all the settings match what we wished to define. If all looks well, we have completed this task!

![](https://academy.hackthebox.com/storage/modules/74/password-6.png)

## Summary

This wraps it up for the first part of the guided lab. We covered how to manage users, groups, and Group Policy. In the next section, we will add a Computer to the INLANEFREIGHT domain, change the OU it exists in, ensuring that it is in the proper group to receive the Group Policy we created earlier.