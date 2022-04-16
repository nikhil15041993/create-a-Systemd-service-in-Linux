

Supervisor is a process manager which provides a singular interface for managing and monitoring a number of long-running programs.

## Step 1 - Installation

```
sudo apt update && sudo apt install supervisor
```

The supervisor service runs automatically after installation. You can check its status:


```
sudo systemctl status supervisor
```

## Step 2 - Adding a Program

In order to demonstrate Supervisor’s functionality, we’ll create a shell script that does nothing other than produce some predictable output once a second, but will run continuously in the background until it is manually stopped. Using vi or your favorite text editor, open a file called idle.sh in your home directory:

```
vi ~/idle.sh
```

Add the following contents:

```
#!/bin/bash
while true
do 
	# Echo current date to stdout
	echo `date`
	# Echo 'error!' to stderr
	echo 'error!' >&2
	sleep 1
done

```

Save and close the file and make your script executable

```
chmod +x ~/idle.sh
```

The per-program configuration files for Supervisor programs are located in the /etc/supervisor/conf.d directory, typically running one program per file and ending in .conf. We’ll create a configuration file for this script, as`/etc/supervisor/conf.d/idle.conf:

```
sudo nano /etc/supervisor/conf.d/idle.conf
```
```
[program:idle]
command=/home/ubuntu/idle.sh
autostart=true
autorestart=true
stderr_logfile=/var/log/idle.err.log
stdout_logfile=/var/log/idle.out.log
```

The autostart option tells Supervisor that this program should be started when the system boots.
Setting this to false will require a manual start following any system shutdown.


autorestart defines how Supervisor should manage the program in the event that it exits:

* false tells Supervisor not to ever restart the program after it exits.
* true tells Supervisor to always restart the program after it exits.
```
stderr_logfile=/var/log/idle.err.log
stdout_logfile=/var/log/idle.out.log
```
The final two lines define the locations of the two main log files for the program.

Once our configuration file is created and saved, we can inform Supervisor of our new program through the supervisorctl command.
First we tell Supervisor to look for any new or changed program configurations in the /etc/supervisor/conf.d 

```
sudo supervisorctl reread
```
Followed by telling it to enact any changes with:
```
sudo supervisorctl update
```

We can check its output by looking at the output log file:
```
sudo tail /var/log/idle.out.log
```

## Step 3 - Managing Programs

To enter the interactive mode, run supervisorctl with no arguments:
```
sudo supervisorctl
```
```
Output
idle                      RUNNING    pid 12614, uptime 1:49:37
supervisor>
```

Entering help will reveal all of its available commands:

```
supervisor> help
```

```
Output
default commands (type help <topic>):
=====================================
add    clear  fg        open  quit    remove  restart   start   stop  update
avail  exit   maintail  pid   reload  reread  shutdown  status  tail  version
```

```
supervisor> stop idle
```

```
supervisor> start idle
```
Using the tail command, you can view the most recent entries in the stdout and stderr logs for your program:
```
supervisor> tail idle
```
Using status you can view again the current execution state of each program after making any changes:
```
supervisor> status
```

Using status you can view again the current execution state of each program after making any changes:
```
supervisor> status
```



## supervisord pros

* Any user can manage processes. No need to be superuser.
* Has nice web interface to manage process.
* Works on any distro
