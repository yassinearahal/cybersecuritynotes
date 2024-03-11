#### Essential Commands:
```css
 ls: Lists the files and directories in the current directory.
 cd: Changes the current directory.
 pwd: Prints the current working directory.
 mkdir: Creates a new directory.
 rm: Removes files and directories.
 cp: Copies files and directories.
 mv: Moves or renames files and directories.
 cat: Displays the contents of a file.
 grep: Searches for a specific pattern in files.
 chmod: Changes the permissions of a file or directory.
 chown: Changes the owner of a file or directory.
 top: Displays real-time information about system processes.
 ps: Lists the running processes.
 kill: Terminates a process.
 ssh: Connects to a remote server using the Secure Shell protocol.
 wget: Downloads files from the web.
 tar: Archives and extracts files.
 df: Displays disk space usage of file systems.
 du: Shows disk usage of files and directories.
 history: Lists the command history.
 man: Displays the manual page for a given command or topic.
 file: determine the type of a file by examining its content.
```

#### Viewing Our processes:

We can use the friendly  <mark style="background: #FF5582A6;">`ps`</mark> command to provide a list of the running processes as our user's session and some additional information such as its status code.

#### User's Processes:

To see the processes run by other users and those that don't run from a session (i.e. system processes), we need to provide **aux** to the `ps` command like so:  <mark style="background: #FF5582A6;">ps aux</mark>

#### Real-Time Processes Statistics:

Another great command to gain insight into our system is via the  <mark style="background: #FF5582A6;">top</mark>  command it gives us real-time statistics about the processes running on our system instead of a one-time view.

#### Managing Processes:

We can send signals that terminate processes; there are a variety of types of signals that correlate to exactly how "cleanly" the process is dealt with by the kernel. To kill a command, we can use the appropriately named  <mark style="background: #FF5582A6;">kill</mark>  command and the associated PID that we wish to kill. i.e., to kill PID 1337, we'd use `kill 1337`.
Below are some of the signals that we can send to a process when it is killed:
-   SIGTERM - Kill the process, but allow it to do some clean-up tasks beforehand
-   SIGKILL - Kill the process - doesn't do any clean-up after the fact
-   SIGSTOP - Stop/suspend a process
<mark style="background: #FF5582A6;">systemctl</mark> -- this command allows us to interact with the <mark style="background: #FF5582A6;">**systemd**</mark> process/daemon. systemctl is an easy to use command. 
##### Syntax:

```bash
systemctl [option] [service]
```

We can do four options with `systemctl`:
-   Start
-   Stop
-   Enable
-   Disable

#### Updating & Upgrading Packages:

APT (Advanced Package Tool) on Ubuntu and Debian-based systems:

```bash
sudo apt update && sudo apt upgrade
```

#### Backgroundig & Forgrounding Processes:

To background a processe we simply use the combination of keys  <mark style="background: #FF5582A6;">Ctrl + Z</mark>  and to foreground it we use the <mark style="background: #FF5582A6;">``fg``</mark> command.

#### Maintaining our System: Automation:

The crontab syntax is used to define scheduled tasks in Unix-like operating systems, including Linux. Each line in a crontab file represents a single task and follows a specific syntax.

Here is the basic syntax for a crontab entry:

```bash
* * * * * command
```

The fields in the crontab entry have the following meanings:

1. Minute (0 - 59): Specifies the minute when the command will run.
2. Hour (0 - 23): Specifies the hour when the command will run.
3. Day of the month (1 - 31): Specifies the day of the month when the command will run.
4. Month (1 - 12): Specifies the month when the command will run.
5. Day of the week (0 - 7, where both 0 and 7 represent Sunday): Specifies the day of the week when the command will run.
6. Command: The command or script that will be executed.

Here are some <mark style="background: #BBFABBA6;">examples</mark> of crontab entries:

```ruby
* * * * * command
0 3 * * * /path/to/script.sh
30 8 * * 1-5 /usr/bin/command
*/15 * * * * /path/to/script.py
```

In the <mark style="background: #BBFABBA6;">examples</mark> above:

- The first entry runs the command every minute.
- The second entry runs the script.sh at 3:00 AM every day.
- The third entry runs the command at 8:30 AM from Monday to Friday.
- The fourth entry runs the script.py every 15 minutes.

Important CronTab switches:

```bash
# Edit the current user's crontab file
crontab -e

# List the current user's crontab entries
crontab -l

# Remove the current user's crontab
crontab -r

# Specify a specific user's crontab to edit, list, or remove
crontab -u <username>

# Prompt for confirmation before removing the current user's crontab
crontab -i

# Check syntax without saving changes (used with -e)
crontab -n
```