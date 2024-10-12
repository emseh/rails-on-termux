# Rails On Termux

Rails is a full-stack framework. It ships with all the tools needed to build amazing web apps on both the front and back end.

Rendering HTML templates, updating databases, sending and receiving emails, maintaining live pages via WebSockets, enqueuing jobs for asynchronous work, storing uploads in the cloud, providing solid security protections for common attacks. Rails does it all and so much more.

*Read [Ruby on Rails](https://rubyonrails.org/) for more details.*

With Rails in termux, you can develop your own projects with your Android.

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

**"Why should install ubuntu?"**

The reason we're going to be using Ubuntu is because the majority of code you write will run on a Linux server. Ubuntu is one of the easiest Linux distributions to use with lots of documentation so it's a great one to start with.

To install ubuntu on termux, I recommend using the [PRoot Distro](https://wiki.termux.com/wiki/PRoot).

PRoot Distro aims to provide all-in-one functionality for managing the installed distributions: installation, de-installation, backup, restore, login. Each action is defined through command. Each command accepts its unique set of options, specific to the task that it performs.

for quick install you can use this command:

`pkg install proot-distro`

then you can continue to install ubuntu use this command:

`proot-distro install ubuntu`

#### Setup Ubuntu

After installing Ubuntu in your Termux via [PRoot Distro](https://wiki.termux.com/wiki/PRoot) you can log in with the following command:

`proot-distro login ubuntu`

Once logged in, you need to install some dependencies on your Ubuntu using this commands:

```
apt update
apt install bash-completion dialog vim nano sudo cron htop curl wget git man-db -y
apt install tzdata neofetch apt-utils libmysqlclient-dev libpq-dev -y
apt upgrade -y
```

Run `dpkg-reconfigure tzdata` if you wish to change *Current default time zone*, *Local time* and *Universal Time*.

#### Create New User on Ubuntu

After installing Ubuntu System, there is only a user you configured during installation except System Accounts and he is an administrative user.
If you'd like to add more common user accounts on System, Configure like follows.

For example, Add a `emseh` user.

Feel free to replace `emseh` with your username.

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

then will show file like below:

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

You can also automatically log in as your created user when running `proot-distro login ubuntu`. To do this, add the following command `su - emseh` to the `~/.bash_login` file in your `proot-distro` Ubuntu system, replacing `emseh` with your username.

Alternatively, you can use the following commands inside `proot-distro` Ubuntu system:

```touch
touch ~/.bash_login
echo "su - emseh" >> ~/.bash_login
```

### Installing Ruby on Rails

**Note:** Please make sure you are using the `ubuntu terminal` in your `proot-distro` for this step.

The following steps are based on the guide from the [GoRails](https://gorails.com/) website. You can follow the instructions there up to the step for Configuring Git. However, for the database setup, we will use a different method.

### Installing Ruby on proot-distro ubuntu

The first step is to install dependencies for compiling Ruby.

Open your `ubuntu terminal` on you `proot-distro` and run the following commands to install them.

```
sudo apt-get update
sudo apt-get install git-core zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev libpq-dev libmysqlclient-dev
```

Next we're going to be installing Ruby using a version manager called [ASDF](https://asdf-vm.com/).

The reason we use ASDF over rbenv, rvm or others is that ASDF can manage other languages like Node.js too.

Installing `asdf` is a simple two step process. First you install `asdf`, and then add it to your shell:

```
cd
git clone https://github.com/excid3/asdf.git ~/.asdf
echo '. "$HOME/.asdf/asdf.sh"' >> ~/.bashrc
echo '. "$HOME/.asdf/completions/asdf.bash"' >> ~/.bashrc
echo 'legacy_version_file = yes' >> ~/.asdfrc
echo 'export EDITOR="code --wait"' >> ~/.bashrc
exec $SHELL
```

Then we can install ASDF plugins for each language we want to use. For Rails, we can install Ruby and Node.js for our frontend Javascript.

```
asdf plugin add ruby
asdf plugin add nodejs
```

To install Ruby and set the default version, we'll run the following commands:

```
asdf install ruby 3.3.5
asdf global ruby 3.3.5

# Update to the latest Rubygems version
gem update --system
```

Confirm the default Ruby version matches the version you just installed.

```
which ruby
#=> /home/username/.asdf/shims/ruby
ruby -v
#=> 3.3.5
```

Then we can install the latest Node.js for handling Javascript in our Rails apps:

```
asdf install nodejs 20.18.0
asdf global nodejs 20.18.0

which node
#=> /home/username/.asdf/shims/node
node -v
#=> 20.18.0

# Install yarn for Rails jsbundling/cssbundling or webpacker
npm install -g yarn

```

### Installing Rails

You can install Rails with the following command. Feel free to change the `version` to the one you prefer:

```
gem install rails -v 8.0.0.beta1
```

Now that you've installed Rails, you can run the `rails -v` command to make sure you have everything installed correctly:

```
rails -v
# Rails 8.0.0.beta1
```

If you get a different result for some reason, it means your environment may not be setup properly.

### Database Setup on Termux

**"What are the reasons for me to install the database on Termux instead of PRoot Distro Ubuntu?"**

Here’s a brief explanation of why you should not install PostgreSQL and MySQL on PRoot Distro Ubuntu:

1. **PostgreSQL** : Fails during bootstrap due to directory ownership issues, preventing proper initialization.
2. **MySQL** : Encounters a "Segmentation fault" when starting the service, while it runs smoothly in Termux using `mysqld_safe`.

These issues indicate that installation on PRoot Distro is unstable, making Termux a better choice.

#### PostgreSQL Installation on Termux

You can install PostgreSQL using the official Termux guide on [https://wiki.termux.com/wiki/Postgresql](https://wiki.termux.com/wiki/Postgresql) or you can follow step below:

Create the skeleton database:

```
mkdir -p $PREFIX/var/lib/postgresql
initdb $PREFIX/var/lib/postgresql
```

Starting the database:

```
pg_ctl -D $PREFIX/var/lib/postgresql start
```

Similarly stop the database using:

```
pg_ctl -D $PREFIX/var/lib/postgresql stop
```

Create User:

Feel free to replace `emseh` with your username.

```
$ createuser --superuser --pwprompt emseh
```

Create your database:

```
$ createdb emseh
```

Open your database:

```
$ psql emseh
```

You will now see the promt:

```
emseh=#
```

If you would like to set a password for the user, you can do the following:

Make sure you are logged into PostgreSQL using the username you created earlier in the Termux terminal:

```
~ $ psql -U emseh
psql (17.0)
Type "help" for help.

emseh=#
```

Then, you can set the password with the following command:

```
emseh=# \password
Enter new password for user "emseh": 
Enter it again: 
emseh=# 

```

#### MySQL/MariaDB Installation on Termux

You can install MySQL/MariaDB using this official termux wiki [https://wiki.termux.com/wiki/MariaDB](https://wiki.termux.com/wiki/MariaDB) or you can follow the steps below:

Install package "mariadb":

`pkg install mariadb `

Whenever you want to access MySQL database manually through command line or with some program (web application), you need to start MySQL server:

`mysqld_safe &`

Then you should be able to connect to database, e.g. with `mysql -u root -p`.

## Troubleshoot

### Phantom Processes Killed "[Process completed (signal 9) - press Enter]" In Android 12 & 13

If you receive a message saying `[Process completed (signal 9) - press Enter]` at seemingly random intervals, and Termux closes once you press Enter, this might occur while trying to install Ruby using a version manager like `asdf`, `rvm`, or `rbenv`.

#### Presquite

**Note:** You need to enable `USB debugging` in the `Developer options` and tools like `adb shell`

This link will guide you through enabling `USB debugging` in `Developer options`:

[https://developer.android.com/studio/debug/dev-options](https://developer.android.com/studio/debug/dev-options).

To enable `adb shell` you need to install `android-tools` on your Termux terminal, use the following command:

`pkg install android-tools`

After the installation is complete, run the command `adb shell` to ensure that `adb shell` is installed. You should see the following message:

```
~ $ adb shell 
* daemon not running; starting now at tcp:5037
* daemon started successfully
adb: no devices/emulators found
```

#### Disable the Phantom Process Killer in android 12 or 13

In here I will guide you to Disable the Phantom Process Killer using Android Wireless debugging on Android 12 & 13.

You will need to use split screen mode on your Android device or use Termux with floating apps to follow these steps.

This the reference video that shows how to use floating/split screen to connect `wireless debugging ` and access the `adb shell`

[https://www.youtube.com/watch?v=KCODAyc_6rU&amp;t=45s
](https://www.youtube.com/watch?v=KCODAyc_6rU&t=45s)

Then, follow these steps:[
](https://www.youtube.com/watch?v=KCODAyc_6rU&t=45s)

1. open `Developer options` > `Enable USB debugging` > `Wireless debugging` > `Enable Wireless debugging` > `Pair device with pairing code`

After this step, your screen may display a popup for "Pair with device" showing the following values:

```
Pair with device
Code: 123456
IP address & Port
192.168.1.10:39677
```

2. In second split screen you need to open your `Termux Terminal` and run following command:

```
~ $ adb pair localhost:39677
Enter pairing code: 123456
Successfully paired to 192.168.1.10:39677 [guid=adb-6328244d-F7iASw]
```

or

```
~ $ adb pair localhost:39677 123456
Successfully paired to 192.168.1.10:39677 [guid=adb-6328244d-F7iASw]
```

3. After successfully paired you need to run this following command:

   **Note:** The `port` should be taken from below your `Device name` in the `Wireless debugging` menu, not from the previous `Pair with device` popup.

```
~ $ adb connect localhost:48965
connected to localhost:48965
```

4. After your device connected you have to run this `adb shell` commands:

   **Note:** Execute the following commands, one after the other.

`adb shell "/system/bin/device_config set_sync_disabled_for_tests persistent"`
`adb shell "/system/bin/device_config put activity_manager max_phantom_processes 2147483647"`
`adb shell settings put global settings_enable_monitor_phantom_procs false`

These commands will disable the phantom process killer. To verify you need to run these two commands one by one.

`adb shell "/system/bin/dumpsys activity settings | grep max_phantom_processes"`
`adb shell "/system/bin/device_config get activity_manager max_phantom_processes"`

Make sure after running these two commands the `output` will look like this:

```
~ $ adb shell "/system/bin/dumpsys activity settings | grep max_phantom_processes"
  max_phantom_processes=2147483647
~ $ adb shell "/system/bin/device_config get activity_manager max_phantom_processes"
2147483647
```

That means the phantom process killer is successfully disabled.

### Handling MySQL common errors

If you got this error when using `rails new project_name -d mysql` and try to run `rails server`

```
/home/emseh/.asdf/installs/ruby/3.3.5/lib/ruby/3.3.0/bundled_gems.rb:75:in `require': libdl.so: cannot open shared object file: No such file or directory - /home/emseh/.asdf/installs/ruby/3.3.5/lib/ruby/gems/3.3.0/gems/mysql2-0.5.6/lib/mysql2/mysql2.so (LoadError)
```

please make sure to install `mysql2` gem with run this command:

`gem install mysql2 `

### Handling MySQL (ActiveRecord::ConnectionNotEstablished)

```
ActiveRecord::ConnectionNotEstablished (Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2))
Caused by: Mysql2::Error::ConnectionError (Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2))

Information for: ActiveRecord::ConnectionNotEstablished (Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)):
```

If you got following error above please make sure to setting your `config/database.yml` by setting `host`, `port`, `username`, and `password` following this example:

make sure to replace `host` value from `"localhost"` to `"127.0.0.1"`.

```
# MySQL. Versions 5.5.8 and up are supported.
#
# Install the MySQL driver
#   gem install mysql2
#
# Ensure the MySQL gem is defined in your Gemfile
#   gem "mysql2"
#
# And be sure to use new-style password hashing:
#   https://dev.mysql.com/doc/refman/5.7/en/password-hashing.html
#
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  host: <%= ENV.fetch("DB_HOST") { "127.0.0.1" } %>

development:
  <<: *default
  database: mysql_development

# Warning: The database defined as "test" will be erased and
# re-generated from your development database when you run "rake".
# Do not set this db to the same as development or production.
test:
  <<: *default
  database: mysql_test

# As with config/credentials.yml, you never want to store sensitive information,
# like your database password, in your source code. If your source code is
# ever seen by anyone, they now have access to your database.
#
# Instead, provide the password or a full connection URL as an environment
# variable when you boot the app. For example:
#
#   DATABASE_URL="mysql2://myuser:mypass@localhost/somedatabase"
#
# If the connection URL is provided in the special DATABASE_URL environment
# variable, Rails will automatically merge its configuration values on top of
# the values provided in this file. Alternatively, you can specify a connection
# URL environment variable explicitly:
#
#   production:
#     url: <%= ENV["MY_APP_DATABASE_URL"] %>
#
# Read https://guides.rubyonrails.org/configuring.html#configuring-a-database
# for a full overview on how database connection configuration can be specified.
#
production:
  <<: *default
  database: mysql_production
  username: mysql
  password: <%= ENV["MYSQL_DATABASE_PASSWORD"] %>

```

### Handling PostgreSQL (ActiveRecord::ConnectionNotEstablished)

```
ActiveRecord::ConnectionNotEstablished (connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: No such file or directory
	Is the server running locally and accepting connections on that socket?
)
Caused by: PG::ConnectionBad (connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: No such file or directory
	Is the server running locally and accepting connections on that socket?
)

Information for: ActiveRecord::ConnectionNotEstablished (connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: No such file or directory
	Is the server running locally and accepting connections on that socket?
):

```

If you got following error above please make sure to setting your `config/database.yml` by setting `host`, `port`, `username`, and `password` following this example:

```
# PostgreSQL. Versions 9.3 and up are supported.
#
# Install the pg driver:
#   gem install pg
# On macOS with Homebrew:
#   gem install pg -- --with-pg-config=/usr/local/bin/pg_config
# On Windows:
#   gem install pg
#       Choose the win32 build.
#       Install PostgreSQL and put its /bin directory on your path.
#
# Configure Using Gemfile
# gem "pg"
#
default: &default
  adapter: postgresql
  encoding: unicode
  # For details on connection pooling, see Rails configuration guide
  # https://guides.rubyonrails.org/configuring.html#database-pooling
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  host: localhost
  port: 5432
  username: emseh
  password: password


development:
  <<: *default
  database: postgresql_development

  # The specified database role being used to connect to PostgreSQL.
  # To create additional roles in PostgreSQL see `$ createuser --help`.
  # When left blank, PostgreSQL will use the default role. This is
  # the same name as the operating system user running Rails.
  #username: postgresql

  # The password associated with the PostgreSQL role (username).
  #password:

  # Connect on a TCP socket. Omitted by default since the client uses a
  # domain socket that doesn't need configuration. Windows does not have
  # domain sockets, so uncomment these lines.
  #host: localhost

  # The TCP port the server listens on. Defaults to 5432.
  # If your server runs on a different port number, change accordingly.
  #port: 5432

  # Schema search path. The server defaults to $user,public
  #schema_search_path: myapp,sharedapp,public

  # Minimum log levels, in increasing order:
  #   debug5, debug4, debug3, debug2, debug1,
  #   log, notice, warning, error, fatal, and panic
  # Defaults to warning.
  #min_messages: notice

# Warning: The database defined as "test" will be erased and
# re-generated from your development database when you run "rake".
# Do not set this db to the same as development or production.
test:
  <<: *default
  database: postgresql_test

# As with config/credentials.yml, you never want to store sensitive information,
# like your database password, in your source code. If your source code is
# ever seen by anyone, they now have access to your database.
#
# Instead, provide the password or a full connection URL as an environment
# variable when you boot the app. For example:
#
#   DATABASE_URL="postgres://myuser:mypass@localhost/somedatabase"
#
# If the connection URL is provided in the special DATABASE_URL environment
# variable, Rails will automatically merge its configuration values on top of
# the values provided in this file. Alternatively, you can specify a connection
# URL environment variable explicitly:
#
#   production:
#     url: <%= ENV["MY_APP_DATABASE_URL"] %>
#
# Read https://guides.rubyonrails.org/configuring.html#configuring-a-database
# for a full overview on how database connection configuration can be specified.
#
production:
  primary: &primary_production
    <<: *default
    database: postgresql_production
    username: postgresql
    password: <%= ENV["POSTGRESQL_DATABASE_PASSWORD"] %>
  cache:
    <<: *primary_production
    database: postgresql_production_cache
    migrations_paths: db/cache_migrate
  queue:
    <<: *primary_production
    database: postgresql_production_queue
    migrations_paths: db/queue_migrate
  cable:
    <<: *primary_production
    database: postgresql_production_cable
    migrations_paths: db/cable_migrate

```

Install Ruby On Rails using Termux.

https://emseh.github.io/rails-on-termux/

References:

https://gorails.com/setup/ubuntu/24.04

https://kskroyal.com/disable-phantom-process-killer-in-android-12-13/

https://wiki.termux.com/wiki/Postgresql

https://wiki.termux.com/wiki/MariaDB

https://www.server-world.info/en/note?os=Ubuntu_24.04&p=initial_conf&f=1
