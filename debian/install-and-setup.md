## Download
[https://www.debian.org/CD/http-ftp/](https://www.debian.org/CD/http-ftp/index.html)  
> Official CD/DVD images of the testing distribution (regenerated weekly)

## Install(netinst)
* Select a language: English - english  
* Select your location: other - Asia - Japan  
* Configure locales: en_US.UTF-8  
* Configure the keyboard: Japanese  
* Configure the network: ...  
* Domain name: ...  
* Set up users and passwords (Password for root): ...  
* Set up users and passwords (Full name for the new user): ...  
* Set up users and passwords (Username for your account): ...  
* Set up users and passwords (Password for user): ...  
* Partition disks: Guided - use entire disk  
* Configure the package manager (Scan extra installation media?): No  
* Configure the package manager (Debian archive mirror country): Japan  
* Configure the package manager (Debian archive mirror): ...  
* Configuring popularity-contest: No  
* Software selection: [Debian desktop environment, GNOME, standard system utilities]  
* Install the GRUB boot loader: Yes - /dev/sda  

## Setup
### Sudoers
Open the sudoers file:
```sh
$ su
$ vi /etc/sudoers
```

Add this:
```text
<user> ALL=(ALL:ALL) ALL
```

### Input Method
```sh
$ sudo aptitude -y install fcitx5-mozc
$ sudo im-config
```
OK -> Yes -> fcitx5 -> OK
```sh
$ sudo reboot
```
```sh
$ fcitx5-configtool
```
Input Method: `["Keyboard - Japanese", "Mozc"]`  
Global Options.Tigger Input Method: `Control + Shift + Space`  

### Run Command Prompt
`[Settings]` - `[Keyboard Shortcuts]` - `[System]` - `[Show the run command prompt]`

### Login Shell
```sh
$ sudo vi /etc/passwd
```
