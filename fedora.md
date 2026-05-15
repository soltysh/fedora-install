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
$ sudo dnf copr enable vgromov/better_fonts
$ sudo dnf install fontconfig fonts-tweak-tool fira-code-fonts bitstream-vera-sans-fonts dejavu-sans-fonts dejavu-sans-mono-fonts dejavu-serif-fonts fontawesome4-fonts gdouros-symbola-fonts google-droid-sans-fonts google-droid-sans-mono-fonts google-noto-emoji-color-fonts levien-inconsolata-fonts liberation-fonts liberation-fonts-common liberation-mono-fonts liberation-sans-fonts liberation-serif-fonts libfonts terminus-fonts
$ sudo dnf install greybird-dark-theme greybird-light-theme adwaita-gtk2-theme adwaita-cursor-theme adwaita-icon-theme
$ sudo dnf install lxappearance pulseaudio-utils mpg123-plugins-pulseaudio xclip
$ sudo dnf install NetworkManager-wifi NetworkManager-openvpn NetworkManager-openvpn-gnome network-manager-applet
$ sudo dnf install vim vim-X11 git tig mercurial hgview make automake gcc g++ gdb hub patch perf sqlite strace tree whois ShellCheck shfmt cronie bison rclone awscli bat v4l-utils bolt
$ sudo dnf install ansible podman buildah htop dmidecode clipit gnome-keyring krb5-workstation tar zip unzip p7zip bzip2 cups pcp-system-tools jq lshw weechat bc rsync mc simple-mtpfs pciutils autofs net-tools rdate usbutils ntfs-3g httpie at bind-utils calibre keepassxc lsof openssl redshift blueman xset btop duf
$ sudo dnf install firefox libreoffice evince
$ sudo dnf install virt-manager bridge-utils libvirt virt-install qemu-kvm
$ sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
$ sudo dnf install ImageMagick ffmpeg ffmpeg-libs gstreamer1-plugin-openh264 mozilla-openh264 geeqie gimp youtube-dl wireshark nmap mtr unrar mplayer pavucontrol inkscape blender darktable audacity obs-studio cmus gimp
$ sudo dnf install lm_sensors smartmontools google-chrome-stable sysstat stress code fzf gh timew rigrep fd-find
$ sudo dnf copr enable hyperreal/better_fonts
$ sudo dnf install archivo-black-fonts catharsis-cormorant-garamond-fonts courier-prime-fonts eosrei-emojione-fonts twemoji-color-font twitter-twemoji-fonts fontconfig-font-replacements impallari-libre-baskerville-fonts ubuntu-fonts
```

### Installing fonts

Follow the steps from https://github.com/soltysh/fedora-better-fonts/blob/master/build_instructions.md to build
packages and then install the results.

### Installing simple-mtpfs

Build and install https://github.com/phatina/simple-mtpfs.

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

### Brother printing

You need to allow cups to execmem, see https://bugzilla.redhat.com/show_bug.cgi?id=1258238#c1:

```sh
setsebool -P cups_execmem 1
```
