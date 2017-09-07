# Install guide for a minimal Fedora 26 desktop with i3

These are my notes for setting up a Fedora 26 installation but with a smaller set of packages.
It's mainly for my own use but it's out there in case anyone else needs it.
It's a fork of https://github.com/benmat/fedora-install but with my own specific modifications.

## Installing the base system

Download [Fedora Server Netinstall ISO](https://getfedora.org/en/server/download/) and [transfer it onto onto a USB-disk](https://docs.fedoraproject.org/f26/install-guide/install/Preparing_for_Installation.html#sect-preparing-boot-media).

In the installer under *Software Selection*, select **Minimal Install**.

To use `sudo` instead of root: once the install starts choose the **User Creation** dialog (with *Make this user administrator* checked), don't use the *Root Password* dialog.

### Graphical environment and necessary tooling

Run these commands to install X, i3 and a terminal.

```sh
$ sudo dnf install @base-x i3 i3lock xbindkeys
$ sudo dnf install abattis-cantarell-fonts dejavu-fonts-common dejavu-sans-fonts dejavu-sans-mono-fonts dejavu-serif-fonts fontconfig fonts-tweak-tool ghostscript-fonts google-crosextra-caladea-fonts google-crosextra-carlito-fonts levien-inconsolata-fonts liberation-fonts-common liberation-mono-fonts liberation-sans-fonts liberation-serif-fonts terminus-fonts urw-fonts
$ sudo dnf install rxvt-unicode bash-completion
$ sudo dnf install NetworkManager-wifi NetworkManager-openvpn network-manager-applet
$ sudo dnf install vim vim-X11 git tig mercurial hgview make gcc gdb golang
$ sudo dnf install ansible docker htop dmidecode clipit gnome-keyring krb5-workstation tar zip unzip p7zip bzip2 cups dstat jq lshw weechat bc rsync mc simple-mtpfs pciutils alsa-utils pulseaudio
$ sudo dnf install firefox libreoffice
$ sudo dnf install virt-manager libvirt-client libvirt-daemon nfs-utils
$ sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
$ sudo dnf install ImageMagick ffmpeg geeqie gimp youtube-dl wireshark nmap mtr unrar mplayer pavucontrol powertop
```

### Setting up environment

```sh
$ git clone https://github.com/soltysh/dotfiles ~/workspace/oth/dotfiles
$ ./config.sh
```

### Setting up xorg to run as a systemd user service after login

Setup xorg as a user service following https://wiki.archlinux.org/index.php/Systemd/User#Xorg_and_systemd:

```sh
$ cat /etc/X11/Xwrapper.config
allowed_users=anybody
needs_root_rights=yes
```

and updating:

```sh
$ cat /etc/systemd/user.conf|grep DefaultEnvironment
DefaultEnvironment=DISPLAY=:0
```

Now you're ready to lunch i3 as a user service:

```sh
$ systemctl --user enable i3.service
$ systemctl --user enable checkbattery.timer
```
