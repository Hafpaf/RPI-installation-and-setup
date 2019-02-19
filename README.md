# RPI-installation-and-setup
Installation and setup for Raspberry PI

## RPI Install notes

### OS
https://www.raspberrypi.org/downloads/raspbian/
Choose 'raspbian strech lite' for no grapics

### Check intregrity of file
sha256sum filename.zip

### Flash: Balena Etcher
https://github.com/balena-io/etcher
- extract and run with:
- cd Downloads
- chmod a+x filename.AppImage
- ./filename.AppImage

### Generate ssh-keys:
https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/
- ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
Enter a file in which to save the key (/home/you/.ssh/id_rsa):
- /home/you/.ssh/id_rsa): filename

Add SSH-key to SSH-agent
- eval "$(ssh-agent -s)"
- ssh-add ~/.ssh/filename

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
