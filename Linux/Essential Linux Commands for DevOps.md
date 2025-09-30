
A curated list of must-know Linux commands for DevOps engineers, covering system management, networking, and container workflows.

## Switch From GUI to Terminal
```bash
  $sudo systemctl set-default graphical.target  #Terminal to GUI
  
  $sudo systemctl set-default multi-user.target   #GUI to Terminal
  ```

## File Management
- `ls` - List directory contents (`dir` equivalent).
  - Use: `ls -l` (long format), `ls -a` (show hidden).
- `cd` - Change directory.
  - Use: `cd /path/to/dir`, `cd ~` (home), `cd ..` (up).
- `cp` - Copy files or directories.
  - Use: `cp source.txt /dest/`, `cp -r dir/ /dest/`.
- `mv` - Move or rename files/directories.
  - Use: `mv oldname newname`, `mv file /new/path/`.
- `rm` - Remove files or directories.
  - Use: `rm file.txt`, `rm -r dir/` (recursive).
- `mkdir` - Create a directory.
  - Use: `mkdir mydir`, `mkdir -p /path/to/nested/dir`.
- `touch` - Create an empty file or update timestamp.
  - Use: `touch newfile.txt`.
- `find` - Search for files/directories.
  - Use: `find / -name "file.txt"`, `find . -type f -mtime -7` (files modified in last 7 days).
- `grep` - Search text in files.
  - Use: `grep "error" log.txt`, `grep -r "pattern" /dir/` (recursive).

## System Monitoring
- `top` - Display real-time system processes.
  - Alternative: `htop` (more user-friendly, if installed).
- `ps` - List running processes.
  - Use: `ps aux` (all processes), `ps -ef | grep app`.
- `df` - Check disk space usage.
  - Use: `df -h` (human-readable).
- `du` - Estimate file/directory size.
  - Use: `du -sh /dir/` (summary in human-readable).
- `free` - Display memory usage.
  - Use: `free -h` (human-readable).
- `uptime` - Show system uptime and load average.
  - Use: `uptime`.

## Permissions & Ownership
- `chmod` - Change file permissions.
  - Use: `chmod 755 script.sh` (rwx for owner, rx for others), `chmod +x file` (executable).
```css
 How to decide the ownership of file
 Numeric Representation

Permissions are often written as numbers (octal):

- r = 4, w = 2, x = 1.
    
- Add them up for each category:
    
    - rwx = 7 (4+2+1).
        
    - r-x = 5 (4+0+1).
        
    - r-- = 4 (4+0+0). Thus, rwxr-xr-- = 754
```
- `chown` - Change file ownership.
  - Use: `chown user:group file.txt`, `chown -R user:group /dir/`.
- `sudo` - Run commands as another user (usually root).
  - Use: `sudo apt update`.
- `ls -l` - To see Ownership of files 
  - Use: `ls -l`

## Package Management
- `apt` - Debian/Ubuntu package manager.
  - Use: `apt update`, `apt install package`, `apt remove package`.
- `yum` / `dnf` - CentOS/RHEL package manager.
  - Use: `yum install package`, `dnf update`.
- `snap` - Universal package manager (if applicable).
  - Use: `snap install app`.

## Networking
- `ping` - Test connectivity to a host.
  - Use: `ping google.com`.
- `curl` - Transfer data from/to a server.
  - Use: `curl http://example.com`, `curl -O file.zip`.
- `wget` - Download files from the web.
  - Use: `wget http://example.com/file.zip`.
- `netstat` - Network statistics (if installed).
  - Use: `netstat -tuln` (listening ports).
- `ss` - Modern replacement for netstat.
  - Use: `ss -tuln` (TCP/UDP listening ports).
- `ifconfig` - Display network interfaces (older systems).
  - Alternative: `ip addr`.
- `ip` - Modern network tool.
  - Use: `ip addr` (interfaces), `ip route` (routing table).
- `ssh` - Securely connect to a remote server.
  - Use: `ssh user@host`, `ssh -i key.pem user@host`.

## Process Management
- `kill` - Terminate a process by PID.
  - Use: `kill 1234`, `kill -9 1234` (forceful).
- `killall` - Kill processes by name.
  - Use: `killall nginx`.
- `jobs` - List background jobs.
  - Use: `jobs`.
- `bg` - Run a stopped job in the background.
  - Use: `bg %1`.
- `fg` - Bring a background job to the foreground.
  - Use: `fg %1`.
- `nohup` - Run a command immune to hangups.
  - Use: `nohup script.sh &`.

## Text Processing
- `cat` - Display or concatenate files.
  - Use: `cat file.txt`, `cat file1 file2 > combined.txt`.
- `less` - View file contents interactively.
  - Use: `less log.txt`.
- `tail` - Show the last lines of a file.
  - Use: `tail -n 10 log.txt`, `tail -f log.txt` (follow live).
- `head` - Show the first lines of a file.
  - Use: `head -n 10 file.txt`.
- `awk` - Process and analyze text.
  - Use: `awk '{print $1}' file.txt` (first column).
- `sed` - Stream editor for text manipulation.
  - Use: `sed 's/old/new/g' file.txt`.

## DevOps-Specific (Containers & Automation)
- `docker` - Manage Docker containers.
  - Use: `docker run`, `docker ps`, `docker build -t name .`.
- `docker-compose` - Manage multi-container apps.
  - Use: `docker-compose up -d`, `docker-compose down`.
- `kubectl` - Control Kubernetes clusters (if applicable).
  - Use: `kubectl get pods`, `kubectl apply -f file.yaml`.
- `systemctl` - Manage system services.
  - Use: `systemctl start nginx`, `systemctl enable docker`.
- `crontab` - Schedule tasks.
  - Use: `crontab -e` (edit), `* * * * * /script.sh` (every minute).

## Miscellaneous
- `man` - Display command manuals.
  - Use: `man ls`.
- `whoami` - Show current user.
  - Use: `whoami`.
- `env` - List environment variables.
  - Use: `env`, `env | grep PATH`.
- `history` - View command history.
  - Use: `history`.
- `tar` - Archive files.
  - Use: `tar -cvf archive.tar /dir/`, `tar -xvf archive.tar`.
- `purge` - Delete the config file of the software
  - Use: step-1  `dpkg --list | grep <keyword>`  
  -         step-2  `sudo apt purge keyword`
---

## Tips
- **Chain Commands**: Use `|` (pipe) to pass output, e.g., `ps aux | grep python`.
- **Redirect**: Use `>` to write to a file, `>>` to append, e.g., `echo "test" > file.txt`.
- **Aliases**: Add shortcuts in `~/.bashrc`, e.g., `alias ll='ls -l'`.


