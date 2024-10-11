# railsontermux.github.io

Install Ruby On Rails using Termux.

https://emseh.github.io/rails-on-termux/

# Rails On Termux

Rails is a full-stack framework. It ships with all the tools needed to build amazing web apps on both the front and back end.

Rendering HTML templates, updating databases, sending and receiving emails, maintaining live pages via WebSockets, enqueuing jobs for asynchronous work, storing uploads in the cloud, providing solid security protections for common attacks. Rails does it all and so much more.

*Read [Ruby on Rails](https://rubyonrails.org/) for more details.*

With Rails in termux, you can develop your own projects with your smartphone or tablet.

## Getting Started with Rails on Termux

### Installing Termux

[Termux](https://github.com/termux/termux-app) is an Android terminal emulator and Linux environment application that works directly with no rooting or setup required. A minimal base system is installed automatically, additional packages are available using the package manager.

Termux application can be obtained from F-Droid from [here](https://f-droid.org/en/packages/com.termux/).

**Notice**: Termux and its plugins are no longer updated on [Google Play](https://play.google.com/store/apps/details?id=com.termux) Store due to [android 10 issues](https://github.com/termux/termux-packages/wiki/Termux-and-Android-10) and have been deprecated. The last version released for Android >= 7 was v0.101. It is highly recommended to not install Termux apps from Play Store any more.

You do not need to download the F-Droid app (via the Download F-Droid link) to install Termux. You can download the Termux APK directly from the site by clicking the Download APK link at the bottom of each version section.

If you have installed termux in your device you can proceed with updating the packages in your termux with command like below:

`pkg update && pkg upgrade -y`

After all packages are updated the next step is to install ubuntu in your termux.

### Install Ubuntu

#### Why should install ubuntu?

The reason we're going to be using Ubuntu is because the majority of code you write will run on a Linux server. Ubuntu is one of the easiest Linux distributions to use with lots of documentation so it's a great one to start with.

To install ubuntu on termux, I recommend using the [PRoot Distro](https://wiki.termux.com/wiki/PRoot).

PRoot Distro aims to provide all-in-one functionality for managing the installed distributions: installation, de-installation, backup, restore, login. Each action is defined through command. Each command accepts its unique set of options, specific to the task that it performs.

for quick install you can use this command:
`pkg install proot-distro`

then you can continue to install ubuntu use this command:
`proot-distro install ubuntu`

##### Setup Ubuntu

After ubuntu installed in your termux via [PRoot Distro](https://wiki.termux.com/wiki/PRoot) you can login with command below:

`proot-distro login ubuntu`

When you finish to login you need install some depedencies on your ubuntu using this command:

```
apt update
apt install bash-completion dialog vim nano sudo cron htop curl wget git man-db -y
apt install tzdata neofetch apt-utils -y
apt upgrade -y
```

Run `dpkg-reconfigure tzdata` if you wish to change *Current default time zone*, *Local time* and *Universal Time*.

##### Create New User on Ubuntu

After installing Ubuntu System, there is only a user you configured during installation except System Accounts and he is an administrative user.
If you'd like to add more common user accounts on System, Configure like follows.

For example, Add a [emseh] user.

```
root@localhost:~# sudo adduser emseh
[sudo] password for ubuntu:  # input your password
info: Adding user `emseh' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `emseh' (1001) ...
info: Adding new user `emseh' (1001) with group `emseh (1001)' ...
info: Creating home directory `/home/emseh' ...
info: Copying files from `/etc/skel' ...
New password:           # set user password
Retype new password:    # confirm
passwd: password updated successfully
Changing the user information for emseh
Enter the new value, or press ENTER for the default
        Full Name []:   # input user info (OK with empty all if you do not need)
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
Is the information correct? [Y/n] y
info: Adding new user `emseh' to supplemental / extra groups `users' ...
info: Adding user `emseh' to group `users' ...
root@localhost:~#
```

Give privileges to a new user, Configure like follows.

```
root@localhost:~# sudo usermod -aG sudo emseh
```

add user to sudoers using command:

`visudo `

then will file like below:

```
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

# This fixes CVE-2005-4890 and possibly breaks some versions of kdesu
# (#1011624, https://bugs.kde.org/show_bug.cgi?id=452532)
Defaults        use_pty

# This preserves proxy settings from user environments of root
# equivalent users (group sudo)
#Defaults:%sudo env_keep += "http_proxy https_proxy ftp_proxy all_proxy no_proxy"

# This allows running arbitrary commands, but so does ALL, and it means
# different sudoers have their choice of editor respected.
#Defaults:%sudo env_keep += "EDITOR"

# Completely harmless preservation of a user preference.
#Defaults:%sudo env_keep += "GREP_COLOR"

# While you shouldn't normally run git as root, you need to with etckeeper
#Defaults:%sudo env_keep += "GIT_AUTHOR_* GIT_COMMITTER_*"

# Per-user preferences; root won't have sensible values for them.
#Defaults:%sudo env_keep += "EMAIL DEBEMAIL DEBFULLNAME"

# "sudo scp" or "sudo rsync" should be able to use your SSH agent.
#Defaults:%sudo env_keep += "SSH_AGENT_PID SSH_AUTH_SOCK"

# Ditto for GPG agent
#Defaults:%sudo env_keep += "GPG_AGENT_INFO"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL
emseh   ALL=(ALL:ALL) ALL # Add this new user

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "@include" directives:

@includedir /etc/sudoers.d

```

and add new user under root previlages

```
# User privilege specification
root    ALL=(ALL:ALL) ALL
emseh   ALL=(ALL:ALL) ALL # Add this new user
```

Login using new user:

```
root@localhost:~# su - emseh
Password:
# try to run a command which requires root privilege
emseh@localhost:~$ sudo ls -la /root
[sudo] password for emseh:     # input self password
total 28
drwx------  4 root root 4096 Apr 26 01:43 .
drwxr-xr-x 23 root root 4096 Apr 26 01:07 ..
-rw-------  1 root root   21 Apr 26 01:43 .bash_history
-rw-r--r--  1 root root 3106 Apr 22 13:04 .bashrc
drwx------  2 root root 4096 Apr 26 01:43 .cache
-rw-r--r--  1 root root  161 Apr 22 13:04 .profile
drwx------  2 root root 4096 Apr 26 01:09 .ssh
```

you can also login automaticaly when run `proot-distro login ubuntu` to your created user to add this command `su - emseh `to `~/.bash_login ` file.
