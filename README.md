# Setup Clean Debian OS for Your (Cloud) VPS

## Step 1. Preparation

 - A clean normally running true virtualization (e.g. KVM) VPS with GRUB2 and VNC access. This script have been tested on SolusVM KVM VPS & Alibaba Cloud ECS with Debian 8/9 & Ubuntu 16.04/18.04.

 - Then check `/etc/default/grub` with your preferred editor (e.g. `nano` or `vi`).

 - Make sure there's **no** `GRUB_HIDDEN_TIMEOUT_QUIET` and `GRUB_HIDDEN_TIMEOUT`. **Just delete them.**

 - Make sure there's reasonable number for `GRUB_DEFAULT` **timeout**. You can just set `GRUB_DEFAULT=999` which will be fine (about 16 minutes).

Install dependencies:

```
sudo apt update && sudo apt -y install ca-certificates cpio wget whois
```

## Step 2. Run the Script

Replace following `<OPTIONS>` with your options.

```
sudo sh -c "$(wget -O - https://github.com/brentybh/debian-netboot/raw/master/netboot.sh)" -- <OPTIONS>
```

**Remember** to enter your current user's password for `sudo` (if need) and then enter the new user's password (if not specified by `-p`).

### All Options

 - `-c US` Debian Installer Country
 - `-fqdn unassigned-hostname.unassigned-domain` FQDN including hostname and domain
 - `-proto http` Transport protocol for archive mirror only but not security repository (`http`, `https`, `ftp`)
 - `-host deb.debian.org` Host for archive mirror only but not security repository
 - `-dir /debian` Directory path relative to root of the mirror
 - `-suite stretch` Suite (`stable`, `testing`, `stretch`, etc.)
 - `-u debian` Username of admin account with sudo privilege
 - `-p secret` Password of the account **(if not specified, it will be asked interactively)**
 - `-tz UTC` [Time zone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List)
 - `-ntp pool.ntp.org` NTP server
 - `-upgrade full-upgrade` Whether to upgrade packages after debootstrap (`none`, `safe-upgrade`, `full-upgrade`)
 - `-s http://security.debian.org/debian-security` Custom URL for security repository mirror
 - `-ip 192.168.1.42` Configure network manually with an IP address (following options only work when IP address specified)
 - `-cidr 255.255.255.0` Netmask for manual network configuration
 - `-gw 192.168.1.1` Gateway for manual network configuration
 - `-ns "8.8.8.8 8.8.4.4"` DNS for manual network configuration
 - `-add "ca-certificates curl fail2ban openssl whois"` Include individual additional packages to install
 - `-ssh secret` Enable network console and specify **password for SSH access during install process**. You can login with `installer` user and check system logs.

### Chinese Special

If `-c CN` is used, Chinese Special options will be setup for good connectivity and experience against GFW.

 - Default archive mirror is `http://ftp.cn.debian.org/debian`.
 - Default security mirror is `http://ftp.cn.debian.org/debian-security`.
 - Default time zone is `Asia/Shanghai`.
 - Default NTP server is `cn.ntp.org.cn`.
 - Default DNS is `156.154.70.5 156.154.71.5`.
 - All custom settings will override above defaults.

## Step 3. Entering Debian Installer

 - Keep your SSH connection and **open VNC console** on your Provider's control panel.
 - `sudo reboot` with your SSH and the VM should **reboot**.
 - Switch to your VNC window **quickly**. You should enter the **GRUB selection menu** now.
 - Use your keyboard to **select** `New Install` and **enter** it. Also, **be quick**, just do not miss the `GRUB_DEFAULT` timeout you've set.
 - Finally, you should see the screen of Debian Installer now. It will setup all things automatically and reboot when complete.
