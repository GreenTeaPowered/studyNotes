# Windows Basics

## 01 -The File System

he file system used in modern versions of Windows is _the New Technology File System or simply NTFS_.

Before NTFS, there was _FAT16/FAT32_ (File Allocation Table) and _HPFS_ (High Performance File System).

You still see FAT partitions in use today. For example, you typically see FAT partitions in USB devices, MicroSD cards, etc. but traditionally not on personal Windows computers/laptops or Windows servers.

NTFS is known as a journaling file system. In case of a failure, the file system can automatically repair the folders/files on disk using information stored in a log file. This function is not possible with FAT.

NTFS addresses many of the limitations of the previous file systems; such as:

- Supports files larger than 4GB
- Set specific permissions on folders and files
- Folder and file compression
- Encryption ( Encryption File System or _EFS_ )

If you're running Windows, what is the file system your Windows installation is using? You can check the Properties (right-click) of the drive your operating system is installed on, typically the C drive (C:\).

Let's speak briefly on some features that are specific to NTFS.

On NTFS volumes, you can set permissions that grant or deny access to files and folders.

The permissions are:

- Full control
- Modify
- Read & Execute
- List folder contents
- Read
- Write

The below image lists the meaning of each permission on how it applies to a file and a folder:

| Permission           | Meaning for Folders                                                                                               | Meaning for Files                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Read                 | Permits viewing and listing of files and subfolders                                                               | Permits viewing or accessing of the file's contents                                   |
| Write                | Permits adding of files and subfolders                                                                            | Permits writing to a file                                                             |
| Read & Execute       | Permits viewing and listing of files and subfolders as well as executing of files; inherited by files and folders | Permits viewing and accessing of the file's contents as well as executing of the file |
| List Folder Contents | Permits viewing and accessing of the file's contents as well as executing of the file                             | N/A                                                                                   |
| Modify               | Permits reading and writing of files and subfolders; allows deletion of the folder                                | Permits reading and writing of the file; allows deletion of the file                  |
| Full Control         | Permits reading, writing, changing, and deleting of files and subfolders                                          | Permits reading, writing, changing and deleting of the file                           |

How can you view the permissions for a file or folder?

- Right-click the file or folder you want to check for permissions.
- From the context menu, select _Properties_.
- Within Properties, click on the _Security_ tab.
- In the _Group or user names_ list, select the user, computer, or group whose permissions you want to view.

Another feature of NTFS is _Alternate Data Streams ( ADS )_.

_Alternate Data Streams (ADS)_ is a file attribute specific to Windows _NTFS_ (New Technology File System).

Every file has at least one data stream (_$DATA_), and ADS allows files to contain more than one stream of data. Natively Window Explorer doesn't display ADS to the user. There are 3rd party executables that can be used to view this data, but Powershell gives you the ability to view ADS for files.

From a security perspective, malware writers have used ADS to hide data.

Not all its uses are malicious. For example, when you download a file from the Internet, there are identifiers written to ADS to identify that the file was downloaded from the Internet.

## 02 - System32 Folders

The Windows folder (_C:\Windows_) is traditionally known as the folder which contains the Windows operating system.

The folder doesn't have to reside in the C drive necessarily. It can reside in any other drive and technically can reside in a different folder.

This is where environment variables, more specifically system environment variables, come into play. Even though not discussed yet, the system environment variable for the Windows directory is **%windir%**.

Per Microsoft , "_Environment variables store information about the operating system environment. This information includes details such as the operating system path, the number of processors used by the operating system, and the location of temporary folders_".

The System32 folder holds the important files that are critical for the operating system.

You should proceed with extreme caution when interacting with this folder. Accidentally deleting any files or folders within System32 can render the Windows OS inoperational.

## 03 - User Accounts, Profiles & Permissions

User accounts can be one of two types on a typical local Windows system: Administrator & Standard User.

The user account type will determine what actions the user can perform on that specific Windows system.

- An Administrator can make changes to the system: add users, delete users, modify groups, modify settings on the system, etc.
- A Standard User can only make changes to folders/files attributed to the user & can't perform system-level changes, such as install programs.

You are currently logged in as an Administrator. There are several ways to determine which user accounts exist on the system.

One way is to click the _Start Menu_ and type _Other User_. A shortcut to _System Settings > Other users_ should appear.

If you click on it, a Settings window should now appear. See below.

Since you're the Administrator, you see an option to Add someone else to this PC.

Note: A Standard User will not see this option.

Click on the local user account. More options should appear: Change account type and Remove.

Click on Change account type. The value in the drop-down box (or the highlighted value if you click the drop-down) is the current account type.

When a user account is created, a profile is created for the user. The location for each user profile folder will fall under is C:\Users.

For example, the user profile folder for the user account Max will be C:\Users\Max.

The creation of the user's profile is done upon initial login. When a new user account logs in to a local system for the first time, they'll see several messages on the login screen. One of the messages, User Profile Service, sits on the login screen for a while, which is at work creating the user profile. See below.

Once logged in, the user will see a dialog box similar to the one below (again), indicating that the profile is in creation.

Each user profile will have the same folders; a few of them are:

- Desktop
- Documents
- Downloads
- Music
- Pictures

Another way to access this information, and then some, is using Local User and Group Management.

Right-click on the Start Menu and click Run. Type _lusrmgr.msc_

Note: The Run Dialog Box allows us to open items quickly.

Back to lusrmgr, you should see two folders: Users and Groups.

If you click on Groups, you see all the names of the local groups along with a brief description for each group.

Each group has permissions set to it, and users are assigned/added to groups by the Administrator. When a user is assigned to a group, the user inherits the permissions of that group. A user can be assigned to multiple groups.

Note: If you click on Add someone else to this PC from Other users, it will open Local Users and Management.

## 04 - User Account Control

The large majority of home users are logged into their Windows systems as local administrators. Remember from the previous task that any user with administrator as the account type can make changes to the system.

A user doesn't need to run with high (elevated) privileges on the system to run tasks that don't require such privileges, such as surfing the Internet, working on a Word document, etc. This elevated privilege increases the risk of system compromise because it makes it easier for malware to infect the system. Consequently, since the user account can make changes to the system, the malware would run in the context of the logged-in user.

To protect the local user with such privileges, Microsoft introduced User Account Control (UAC). This concept was first introduced with the short-lived Windows Vista and continued with versions of Windows that followed.

Note : UAC (by default) doesn't apply for the built-in local administrator account.

How does UAC work? When a user with an account type of administrator logs into a system, the current session doesn't run with elevated permissions. When an operation requiring higher-level privileges needs to execute, the user will be prompted to confirm if they permit the operation to run.

Let's look at the program on the account you're currently logged into, the built-in administrator account—Right-click to view its Properties.

In the Security tab, we can see the users/groups and their permissions to this file. Notice that the standard user is not listed.

Log in as the standard user and try to install this program. To do this, you can remote desktop into the machine as the standard user account.

Note : You have the username and password for the standard user. It's visible in lusrmgr.msc .

Before installing the program, notice the icon. Do you see the difference? When you're logged in as the standard user, the shield icon is on the program's default icon. See below.

This shield icon is an indicator that UAC will prompt to allow higher-level privileges to install the program.

Double-click the program, and you'll see the UAC prompt. Notice that the built-in administrator account is already set as the user name and prompts the account's password. See below.

After some time, if a password is not entered, the UAC prompt disappears, and the program does not install.

This feature reduces the likelihood of malware successfully compromising your system. You can read more about UAC here.

## 05 - Settings And The Control Panel

On a Windows system, the primary locations to make changes are the Settings menu and the Control Panel.

For a long time, the Control Panel has been the go-to location to make system changes, such as adding a printer, uninstall a program, etc.

The Settings menu was introduced in Windows 8, the first Windows operating system catered to touch screen tablets, and is still available in Windows 10. As a matter of fact, the Settings menu is now the primary location a user goes to if they are looking to change the system.

There are similarities and differences between the two menus. Below are screenshots of each.

Note : The icons for Settings might be different in the version of Windows on your personal device.

Both can be accessed from the Start Menu. See below.

Control Panel is the menu where you will access more complex settings and perform more complex actions. In some cases, you can start in Settings and end up in the Control Panel.

For example, in Settings, click on Network & Internet . From here, click on Change adapter options .

Notice that the next window that pops up is from the Control Panel.

If you're unclear which to open if you wish to change a setting, use the Start menu and search for it.

In the example below, the search was 'wallpaper.' Notice that few results were returned.

If we click on the Best match, a window to the Settings menu appears to make changes to the wallpaper.

## 06 - Task Manager

The last subject that will be touched on in this module is the Task Manager.

The Task Manager provides information about the applications and processes currently running on the system. Other information is also available, such as how much CPU and RAM are being utilized, which falls under Performance.

You can access the Task Manager by right-clicking the taskbar.

Task Manager will open in Simple View and won't show much information.

Click on More details, and the view changes.

You can refer to this blog post for more detailed information about the Task Manager.

If you wish to learn more about the core Windows processes and what each process is responsible for, visit the Core Windows Processes room.
