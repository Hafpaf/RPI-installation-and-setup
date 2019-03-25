# RPI-installation-and-setup
SSH-Key generation + installation and setup for Raspberry PI

This guide was made for myself, but can of course help others.

Please note that the OS on the host computer is running Arch and may not be the exact same on your system.

## RPI Install notes

### OS
Download link: [https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/ "Raspian download")

Choose `raspbian stretch lite`, to go run without graphics and preinstalled programs.

#### Check integrity of file
`sha256sum filename.zip`

#### Flash: Balena Etcher
Download link: [https://github.com/balena-io/etcher](https://github.com/balena-io/etcher "Etcher")

> NOTE: The old Etcher have changed to Balena etcher due to the company changing names.


Extract and run with:
```bash
cd Downloads
chmod a+x filename.AppImage
./filename.AppImage
```

- Run Balena Etcher
- Plug in SD-card
- Flash
- Plug SD-card into RPI

### Generate ssh-keypair:
For more security, we will generate a ssh-keypair, install the public key on remote computer and disable password login.

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
[https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md "Enable SSH")

Create a file called `ssh` with no filetype, at the root folder of boot partion.

### Boot RPI
- Plug in LAN cable to router and PI
- Plug in power

### SSH over to RPI
```bash
ssh pi@host
Password: raspberry
```
Find IP adress of the RPI by logging in to your router.

Default password is `raspberry` and will be changed.

Add the ip to known host by typing `yes`

### Change Password
`passwd`

### Add user
[https://www.raspberrypi.org/documentation/linux/usage/users.md](https://www.raspberrypi.org/documentation/linux/usage/users.md "Add user")
```bash
sudo adduser user_name
```

### Add ssh-key for passwordless login:
[https://www.ssh.com/ssh/copy-id](https://www.ssh.com/ssh/copy-id "Copy public key to RPI")

Exit the ssh session with `exit`

```bash
ssh-copy-id -i ~/.ssh/rpi.pub user@host
```
Test key
```bash
ssh -i ~/.ssh/rpi.pub user@host
```

### Disable password login
SSH over to RPI

```bash
sudo nano /etc/ssh/sshd_config
```
Change `#PasswordAuthentication yes` to `PasswordAuthentication no`

Restart ssh service on RPI
```bash
sudo service ssh restart
```

### Update the RPI
```bash
sudo -- sh -c 'apt-get update; apt-get upgrade -y; apt-get dist-upgrade -y; apt-get autoremove -y; apt-get autoclean -y'
sudo reboot
```
## Extra
### Install Pi-Hole
[https://github.com/pi-hole/pi-hole/#one-step-automated-install](https://github.com/pi-hole/pi-hole/#one-step-automated-install "Install Pi-Hole")

```bash
curl -sSL https://pi-hole.net | bash
```

> Change DNS server on Router or each device to the ip adress of the RPI

### Give user sudo rights
```bash
sudo adduser username sudo
```

### Set static ip
```bash
sudo nano /etc/dhcpcd.conf
```

## Credits
Written with help of this [Markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
