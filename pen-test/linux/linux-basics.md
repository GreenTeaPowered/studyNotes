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

## 03 - General/Useful Utilities

### Curl & Wget

Download files from the internet via http(s)

When using the commands as shown below, _curl_ will only grab the contents of the url.
So if this is a text file for example, it will only grab the content and display it in your terminal.
Where as _wget_ will actually download the file to your current directory.

`wget "url"`

`curl "url"`

### SCP

Secure copy, or SCP, is just that -- a means of securely copying files. Unlike the regular cp command, this command allows you to transfer files between two computers using the SSH protocol to provide both authentication and encryption.

- Copy files & directories from your current system to a remote system
- Copy files & directories from a remote system to your current system

The context for the example below:

- The IP address of the remote system > 192.168.1.30
- User on the remote system > ubuntu
- Name of the file on the local system > important.txt
- Name that we wish to store the file as on the remote system > transferred.txt

`scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt`

And the example below will show the reverse:

- The IP address of the remote system > 192.168.1.30
- User on the remote system > ubuntu
- Name of the file on the local system > important.txt
- Name that we wish to store the file as on the remote system > transferred.txt

`scp ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt`

### Serving Files From Your Host - WEB

Ubuntu machines come pre-packaged with _python3_. Python helpfully provides a lightweight and easy-to-use module called _"HTTPServer"_. This module turns your computer into a quick and easy web server that you can use to serve your own files, where they can then be downloaded by another computing using commands such as _curl_ and _wget_.

_Python3's "HTTPServer"_ will serve the files in the directory where you run the command, but this can be changed by providing options that can be found within the manual pages. Simply, all we need to do is run `python3 -m  http.server`.

## 04 - Processes 101

Processes are the programs that are running on your machine. They are managed by the kernel, where each process will have an ID associated with it, also known as its PID. The PID increments for the order In which the process starts. I.e. the 60th process will have a PID of 60.

### PS

We can use the friendly `ps` command to provide a list of the running processes as our user's session and some additional information such as its status code, the session that is running it, how much usage time of the CPU it is using, and the name of the actual program or command that is being executed.

o see the processes run by other users and those that don't run from a session (i.e. system processes), we need to provide aux to the ps command like so:

`ps aux`

### TOP

Another very useful command is the `top` command. _top_ gives you real-time statistics about the processes running on your system instead of a one-time view. These statistics will refresh every 10 seconds, but will also refresh when you use the arrow keys to browse the various rows. Another great command to gain insight into your system is via the _top_ command.

### KILL

You can send signals that terminate processes. There are a variety of types of signals that correlate to exactly how "cleanly" the process is dealt with by the kernel. To _kill_ a command, we can use the appropriately named `kill` command and the associated _PID_ that we wish to kill. i.e., to kill _PID_ 1337, we'd use `kill 1337`.

Below are some of the signals that we can send to a process when it is killed:

SIGTERM - Kill the process, but allow it to do some cleanup tasks beforehand
SIGKILL - Kill the process - doesn't do any cleanup after the fact
SIGSTOP - Stop/suspend a process

### Namespaces

Let's start off by talking about namespaces. The Operating System (OS) uses namespaces to ultimately split up the resources available on the computer to (such as CPU, RAM and priority) processes. Think of it as splitting your computer up into slices -- similar to a cake. Processes within that slice will have access to a certain amount of computing power, however, it will be a small portion of what is actually available to every process overall.

Namespaces are great for security as it is a way of isolating processes from another -- only those that are in the same namespace will be able to see each other.

We previously talked about how PID works, and this is where it comes into play. The process with an ID of 0 is a process that is started when the system boots. This process is the system's init on Ubuntu, such as _systemd_, which is used to provide a way of managing a user's processes and sits in between the operating system and the user.

For example, once a system boots and it initialises, _systemd_ is one of the first processes that are started. Any program or piece of software that we want to start will start as what's known as a child process of _systemd_. This means that it is controlled by _systemd_, but will run as its own process (although sharing the resources from _systemd_) to make it easier for us to identify and the likes.

### Systemctl

Some applications can be started on the boot of the system that we own. For example, web servers, database servers or file transfer servers. This software is often critical and is often told to start during the boot-up of the system by administrators.

In this example, we're going to be telling the apache web server to be starting apache manually and then telling the system to launch apache2 on boot.

Enter the use of _systemctl_ -- this command allows us to interact with the _systemd_ process/daemon. Continuing on with our example, _systemctl_ is an easy to use command that takes the following formatting:

`systemctl [option] [service]`

For example, to tell apache to start up, we'll use `systemctl start apache2`. Seems simple enough, right? Same with if we wanted to stop apache, we'd just replace the [option] with stop (instead of start like we provided)

We can do four options with _systemctl_:

- Start
- Stop
- Enable
- Disable

### Processes In For or Background

Processes can run in two states: In the background and in the foreground. For example, commands that you run in your terminal such as _"echo"_ or things of that sort will run in the foreground of your terminal as it is the only command provided that hasn't been told to run in the background. _"Echo"_ is a great example as the output of echo will return to you in the foreground.

But if we want to run a program in the background so we can still use our current terminal without it be locked/frozen we can add the _&_ to the command.
An example of this would be to open up a browser window like _firefox_, if you run this application in the terminal. The process for running the program will take control or lock up your current terminal session as it is active in the foreground. If we do the following command:

`firefox &`

It will still be tied to your current terminal session, but it will be assigned a PID and run in the background, so you can still freely use your current terminal session.

If you want to _disown_ the process from your current terminal session you can also add the _disown_ keyword after the _&_, like so:

`firefox & disown`

If you would kill your terminal session, your _firefox_ process will keep running as well.

### Return To Foreground

To bring a background process back to the foreground you can use the following command:

`fg`

## 06 - Maintaining Your System: Automation

Users may want to schedule a certain action or task to take place after the system has booted. Take, for example, running commands, backing up files, or launching your favourite programs on, such as Spotify or Google Chrome.

We're going to be talking about the _cron_ process, but more specifically, how we can interact with it via the use of _crontabs_. _Crontab_ is one of the processes that is started during boot, which is responsible for facilitating and managing cron jobs.

A crontab is simply a special file with formatting that is recognised by the cron process to execute each line step-by-step. Crontabs require 6 specific values:

- MIN > What minute to execute at
- HOUR >What hour to execute at
- DOM > What day of the month to execute at
- MON > What month of the year to execute at
- DOW > What day of the week to execute at
- CMD > The actual command that will be executed.

Let's use the example of backing up files. You may wish to backup "cmnatic"'s "Documents" every 12 hours. We would use the following formatting:

`0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/`

An interesting feature of _crontabs_ is that these also support the _wildcard_ or asterisk (\*). If we do not wish to provide a value for that specific field, i.e. we don't care what month, day, or year it is executed -- only that it is executed every 12 hours, we simply just place an asterisk.

This can be confusing to begin with, which is why there are some great resources such as the online "Crontab Generator" that allows you to use a friendly application to generate your formatting for you! As well as the site "Cron Guru"!

_Crontabs_ can be edited by using `crontab -e`, where you can select an editor (such as Nano) to edit your _crontab_.

## 07 - Maintaining Your System: Package Management

When developers wish to submit software to the community, they will submit it to an "apt" repository. If approved, their programs and tools will be released into the wild. Two of the most redeeming features of Linux shine to light here: User accessibility and the merit of open source tools.

When using the `ls` command in the _/etc/apt/_ directory, on a Ubuntu 20.04 Linux machine, these files serve as the gateway/registry.

Whilst Operating System vendors will maintain their own repositories, you can also add community repositories to your list! This allows you to extend the capabilities of your OS. Additional repositories can be added by using the `add-apt-repositorycommand` or by listing another provider! For example, some vendors will have a repository that is closer to their geographical location.

### Managing Your Repositories (Adding & Removing)

Normally we use the apt command to install software onto our Ubuntu system. The `apt` command is a part of the package management software also named apt. Apt contains a whole suite of tools that allows us to manage the packages and sources of our software, and to install or remove software at the same time.

One method of adding repositories is to use the `add-apt-repository` command we illustrated above, but we're going to walk through adding and removing a repository manually. Whilst you can install software through the use of package installers such as `dpkg`, the benefits of apt means that whenever we update our system -- the repository that contains the pieces of software that we add also gets checked for updates.

In this example, we're going to add the text editor Sublime Text to our Ubuntu machine as a repository as it is not a part of the default Ubuntu repositories. When adding software, the integrity of what we download is guaranteed by the use of what is called GPG (Gnu Privacy Guard) keys. These keys are essentially a safety check from the developers saying, "here's our software". If the keys do not match up to what your system trusts and what the developers used, then the software will not be downloaded.

Example of this process would look like this:

1 - Let's download the GPG key and use apt-key to trust it:

`wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -`

2 - Now that we have added this key to our trusted list, we can now add Sublime Text 3's repository to our apt sources list. A good practice is to have a separate file for every different community/3rd party repository that we add.

2.1 - Let's create a file named sublime-text.list in /etc/apt/sources.list.d and enter the repository information like so:

`touch sublime-text.list`

2.2 - And now use Nano or a text editor of your choice to add & save the Sublime Text 3 repository into this newly created file:

`deb https://download.sublimetext.com/ apt/stable/`

2.3 - After we have added this entry, we need to update apt to recognise this new entry -- this is done using the `apt update` command.

2.4 - Once successfully updated, we can now proceed to install the software that we have trusted and added to apt using `apt install sublime-text`

Removing packages is as easy as reversing. This process is done by using the `add-apt-repository --remove ppa:PPA_Name/ppa` command or by manually deleting the file that we previously added to. Once removed, we can just use `apt remove [software-name-here]` i.e. `apt remove sublime-text`

## 08 - Maintaining Your System: Logs

Located in the /var/log directory, these files and folders contain logging information for applications and services running on your system. The Operating System (OS) has become pretty good at automatically managing these logs in a process that is known as "rotating".

I have highlighted some logs from three services running on a Ubuntu machine:

- An Apache2 web server
- Logs for the fail2ban service, which is used to monitor attempted brute forces, for example
- The UFW service which is used as a firewall

These services and logs are a great way in monitoring the health of your system and protecting it. Not only that, but the logs for services such as a web server contain information about every single request - allowing developers or administrators to diagnose performance issues or investigate an intruder's activity. For example, the two types of log files below that are of interest:

- access log
- error log

There are, of course, logs that store information about how the OS is running itself and actions that are performed by users, such as authentication attempts.
