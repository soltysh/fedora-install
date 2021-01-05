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
$ sudo dnf install @base-x i3 i3lock xbindkeys rxvt-unicode bash-completion
$ sudo dnf copr enable dawid/better_fonts
$ sudo dnf copr enable vbatts/bazel
$ sudo dnf install fontconfig fonts-tweak-tool fontconfig-font-replacements bitstream-vera-sans-fonts blender-fonts courier-prime-fonts dejavu-sans-fonts dejavu-sans-mono-fonts dejavu-serif-fonts eosrei-emojione-fonts fontawesome-fonts gdouros-symbola-fonts gelasio-fonts google-droid-sans-fonts google-droid-sans-mono-fonts google-noto-emoji-color-fonts levien-inconsolata-fonts liberation-fonts liberation-fonts-common liberation-mono-fonts liberation-sans-fonts liberation-serif-fonts libfonts libre-baskerville-fonts terminus-fonts
$ sudo dnf install greybird-dark-theme greybird-light-theme adwaita-gtk2-theme adwaita-cursor-theme adwaita-icon-theme
$ sudo dnf install lxappearance pulseaudio-utils alsa-plugins-pulseaudio mpg123-plugins-pulseaudio xclip
$ sudo dnf install NetworkManager-wifi NetworkManager-openvpn NetworkManager-openvpn-gnome network-manager-applet
$ sudo dnf install vim vim-X11 git tig mercurial hgview make gcc gdb bazel hub patch perf sqlite strace tree whois ShellCheck cronie bison
$ sudo dnf install ansible podman buildah htop dmidecode clipit gnome-keyring krb5-workstation tar zip unzip p7zip bzip2 cups dstat jq lshw weechat bc rsync mc simple-mtpfs pciutils alsa-utils pulseaudio autofs net-tools rdate usbutils ntfs-3g httpie at bind-utils calibre keepassx lsof openssl
$ sudo dnf install firefox libreoffice evince
$ sudo dnf install virt-manager bridge-utils libvirt virt-install qemu-kvm
$ sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
$ sudo dnf install ImageMagick ffmpeg geeqie gimp youtube-dl wireshark nmap mtr unrar mplayer pavucontrol inkscape blender darktable audacity openshot
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
