# RPI-installation-and-setup
SSH-Key encryption, installation and setup for Raspberry PI

This guide was made for myself, but can of course help others.

Please note that the OS on the host computer is running Arch and may not be the exact same on your system.

## RPI Install notes

### OS
Download link: [https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/ "Raspian download")

Choose `raspbian strech lite`, to go run without grapichs and preinstalled programs.

### Check intregrity of file
`sha256sum filename.zip`

### Flash: Balena Etcher
Download link: [https://github.com/balena-io/etcher](https://github.com/balena-io/etcher "Etcher")

> NOTE: The old Etcher have changed to Balena etcher due to the company changeing names.


Extract and run with:
```bash
cd Downloads
chmod a+x filename.AppImage
./filename.AppImage
```

### Generate ssh-keypair:
Guide followed: [https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/ "Generate SSH-keypair")

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
Enter a file in which to save the key (/home/you/.ssh/id_rsa):
```bash
/home/you/.ssh/id_rsa): folder/path/filename
```

#### Add SSH-key to SSH-agent
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/filename
```

### Enable SSH on headless boot
https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
Add a file called "ssh" with no filetype on root folder of boot partion.

### Boot RPI
Plug in LAN cable to router and PI
Plug in power

### SSH over to RPI
- ssh pi@ip_address
- Password: raspberry
NOTE: Add the ip to known host by typing "yes"

### Change Password
- passwd

### Add user
https://www.raspberrypi.org/documentation/linux/usage/users.md
- sudo adduser user_name

### Add ssh-key for passwordless login:
https://www.ssh.com/ssh/copy-id
Exit the ssh session with "exit"
- ssh-copy-id -i ~/.ssh/rpi.pub user@host
Test key
- ssh -i ~/.ssh/rpi.pub user@host

### Disable password login
- sudo nano /etc/ssh/sshd_config
Find "#PasswordAuthentication yes"
Change to "PasswordAuthentication no"
- sudo service ssh restart

### Update the rpi
- sudo -- sh -c 'apt-get update; apt-get upgrade -y; apt-get dist-upgrade -y; apt-get autoremove -y; apt-get autoclean -y'
- sudo reboot

### Install Pi-Hole
https://github.com/pi-hole/pi-hole/#one-step-automated-install
- curl -sSL https://pi-hole.net | bash

## Credits
Written with help of this [cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
