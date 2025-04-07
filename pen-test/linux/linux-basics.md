# Linux Basics

With this I mean tools that are on the Linux system by default

## 01 - Basics Syntax & Commands

### Searching for Files

#### Find

`find` is a simple search command

Example:

`find -n *.txt`

Flags:

`-n` is the flag for the filename we are looking for, in the example above, anything with the `.txt` extension

#### Grep

We can use `grep` to search the entire contents of a file like so:

`grep "search value" access.log`

In this example the text in quotes is the value we will look for in the `access.log` file.

### Shell Operators

`&` = This operator allows you to run commands in the background of your terminal session
`&&` = This operator allows you to combine multiple commands together in one line of your terminal

`>` = This operator is a redirector, it can take the output from a command and direct it elsewhere

`>>` = This operator does the same as the `>` operator but appends the output rather replace (meaning nothing is overwritten)

## 02 - Linux Directories

### /etc

This root directory is one of the most important root directories on your system.
The `etc` folder (short for etcetera) is a commonplace location to store system files that are used by your operating system.

### /var

The var directory, with var being short for variable data, is one of the main root folders found on a Linux install. This folder stores data that is frequently accessed or written to by services or applications on the system. For example, log files from running services and applications are written here (/var/log), or data that is not necessarily associated with a specific user (i.e., databases and the like)

### /root

The actual root directory for the system. There isn't anything more to this folder other than just understanding that this is the home directory for the "root" user. But, it is worth a mention as the logical presumption is that this user would have their data in a directory as "/home/root" by default.

### /tmp

This is a unique root directory found on a Linux install. Short for `temporary`, the tmp directory is volatile and is used to store data that is only needed to be accessed once or twice. Similar to the memory on your computer, once the computer is restarted, the content of this folder is cleared out.

What is useful in pen-testing is that any user can write to this folder by default. Meaning once we have access to a machine, it servers as a good place to store things like our enumeration scripts.
