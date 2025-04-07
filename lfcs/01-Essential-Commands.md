# Linux Study Notes

## Hard links

Hard links allow you to share files in multiple location without copying the file as a whole.

Files in Linux have something called an **_Inode ID_**, which the system uses to keep track of where a file is on the _file system_.

So if you want two users to have access to the same file that is located in the home directory of 1 user for example.
The user or admin could create a "hard link" to the file for the second user.

This will allow the second user to have their own _"copy"_ of the file in their own home directory
I say "copy" in quotation marks here as it is the same file in the file system, but the second user can rename and move the file as they please.
The file will keep the same **_Inode ID_** so the system knows what file you are referring to when you accessing it.

You can create an hard link with the following syntax:

`ln /path/to/target/file /path/to/new/link/location`

When using the following command

`stat /path/to/file`

You can see the **_Inode ID_** as well the amount of (Hard) **_Links_** the file has.
As well some other great info like when the file was created, last changed, accessed or modified.
And what type of permissions are on the file.

### Limitations

- 1 - Files only

You can only create hard links to _files_ not to _directories_

- 2 - Same file system only

And the files need to be on the same _file system_

So if you had an external hard drive mounted to `/mnt/backups` for example.
You would not be able to hard link a file to/from your external hard drive.
As these are different _file systems_

- 3 - Same file permissions required

All users that have a hard link to the file.
Need to have the same permissions to that file.

## Soft Link / Symbolic Link

You can think of a _soft link_ as a shortcut as you know it on mac/windows.
It is a reference to a file and does **not** have its own **_Inode ID_**.

You can create a _soft link_ with the following command:

`ln -s /path/to/target/file /soft/link/shortcut/path`

### Good To Know

- 1 - ls information

When using the `ls -l` command if you see an _l (L)_ in front of the permissions line.
This means the item/file you are looking at is a _soft link_

- 2 - Full path command

In case the _soft link_ file path is to long that it does not fit on the `ls -l` command
You can read the _soft link_ with the following command:

`readlink <soft-link file/path>`

- 3 - Examples

In the course a picture of a dog is used in their examples.
The original file is _family_dog.jpg_, and they created a _soft link_ with the following name _family_dog_shortcut.jpg_.

For the `ln -s` and `readlink` commands would look like this:

`ln -s /home/john/pictures/family_dog.jpg family_dog_shortcut.jpg`

`readlink family_dog_shortcut.jpg`

- 4 - File systems

_Soft links_ **can** be used with different file systems.
