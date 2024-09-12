# Ubuntu-Server-optimization
# Tuning Ubuntu Server for Performance

You are having an Ubuntu cloud server and it is running slow? There may be something wrong with your code or (and) something wrong with your server configuration. Sometimes I realize that my server is slow just because I haven’t done any configuration for tuning it.

So, I write this note to share some simple tricks and must-do to help your server run with its maximum performance.

## Increase File Descriptor Limit (`ulimit`)

The file descriptor limit might be too low by default. For example, on a new DigitalOcean droplet, it is often set to 1024. If you are running an application server that receives thousands or millions of requests per second, you need to increase this value.

### Step 1: Modify `/etc/security/limits.conf`

Add the following lines:

```bash
*    soft nofile 1048576
*    hard nofile 1048576
root soft nofile 1048576
root hard nofile 1048576
```
### Step 2: Modify /etc/pam.d/common-session

Add the following line to ensure limits are applied to all sessions:
```bash
session required pam_limits.so

```
### Step 3: Modify /etc/sysctl.conf

Add the following lines to set system-wide file limits:
```
fs.file-max = 2097152
fs.nr_open = 1048576

```
### Then reload the changes using:
```
sudo sysctl -p
```
### Step 4: Reboot and Verify

Reboot the system:
```
sudo reboot
```
After rebooting, check the new file descriptor limit with:
```
ulimit -n
```
