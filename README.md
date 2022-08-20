# *nix Annoyances

### Change default terminal text editor

Add the following lines to your .bashrc file

```
EDITOR='nano'
VISUAL='nano'
```

You can change nano with anything else

$ `source .bashrc`

### Change git default editor

$ `git config --global core.editor "nano"`

You can change nano with anything else

### Disable anaconda environment

$ `conda config --set auto_activate_base disable`

It can be enabled again by changing disable to enable

### Reduce swapiness

$ `sudo nano /etc/sysctl.conf`

Add the following line to the end of file

`vm.swappiness = 10`

$ `sudo sysctl -p && sudo sysctl vm.swappiness=10`

### Speed up and reduce CPU usage in KDE Plasma 5 on systems with HDD

$ `balooctl disable`

### Dolphin giving pain with MTP i.e. Giving error when trying to access files on MTP device (workaround, it will still give pain!)

Unplug device, close all all tabs of dolphin and then dolphin itself then run the following command

$ `sudo udevadm trigger --action=change`

Launch dolphin again.

### Fix KDE Application Launcher not responding to Super/ Meta Key

Right Click on Application Launcher Icon > Configure Application Launcher > Keyboard Shortcuts > Set shortcut as `ALT + F1` > Apply

### Enable backspace key in Nautilus

$ `sudo dnf install nautilus-python -y`

$ `sudo apt install python-nautilus -y`

```
mkdir -p ~/.local/share/nautilus-python/extensions && cd ~/.local/share/nautilus-python/extensions && wget https://raw.githubusercontent.com/riclc/nautilus_backspace/master/BackspaceBack.py && killall nautilus
```

### Speed up SSD

$ `sudo apt install hdparm`

$ `sudo dnf install hdparm`

$ `sudo hdparm -B254 /dev/sda`

### RTL8723BE PCIe Wireless Network Adapter stops working or becomes unusable

This happens because of powersaving mode of `NetworkManager`

$ `sudo nano /etc/modprobe.d/50-rtl8723be.conf`

Add the following lines

```
options rtl8723be ips=0 fwlps=0
```

$ `sudo mkdir -p /etc/NetworkManager/conf.d/`

$ `sudo nano /etc/NetworkManager/conf.d/wifi-powersave-off.conf`

Add the following lines

```# File to be place under /etc/NetworkManager/conf.d
[connection]
# Values are 0 (use default), 1 (ignore/don't touch), 2 (disable) or 3 (enable).
wifi.powersave = 2
```

$ `sudo nano /etc/NetworkManager/conf.d/wifi-powersave-on.conf`

Add the following lines

```
# File to be place under /etc/NetworkManager/conf.d
[connection]
# Values are 0 (use default), 1 (ignore/don't touch), 2 (disable) or 3 (enable).
wifi.powersave = 3
```

Finally reboot.

### Wi-Fi and Bluetooth not working simultaneously

$ `sudo tee /etc/modprobe.d/iwlwifi-opt.conf <<< "options iwlwifi bt_coex_active=N"`

Finally reboot.

### Enable optional features in Gnome Boxes

While in guest OS, run

$ `sudo apt install spice-webdavd -y && reboot`

$ `sudo dnf install spice-webdavd -y && reboot`

## Ubuntu

### Enable ZRAM

$ `sudo apt install zram-config`

### Installing plugins for playing movies and music

```
curl https://download.videolan.org/pub/debian/videolan-apt.asc | sudo apt-key add - && echo "deb https://download.videolan.org/pub/debian/stable ./" | sudo tee /etc/apt/sources.list.d/libdvdcss.list && sudo add-apt-repository multiverse && sudo apt update && sudo apt install ubuntu-restricted-extras curl vlc vlc-data browser-plugin-vlc mplayer2
```

## Fedora

### Increase DNF download speed

$ `sudo nano /etc/dnf/dnf.conf`

Add the following line in the of the file

`max_parallel_downloads=10`

### Installing plugins for playing movies and music

```
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm -y && sudo dnf install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm -y && sudo dnf install gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel vlc lame\* --exclude=lame-devel && sudo dnf group upgrade --with-optional Multimedia -y
```

### Install KDE minus the garbage packages

```
sudo dnf install @kde-desktop --exclude=akregator --exclude=kwrite --exclude=kruler --exclude=kmouth --exclude=kmousetool --exclude=kmag --exclude=kde-partitionmanager --exclude=kcolorchooser --exclude=kcharselect --exclude=kfind -y
```

### Remove garbage packages from Fedora KDE spin/ @kde-desktop

$ `sudo dnf remove akregator kwrite kruler kmouth kmousetool kmag kde-partitionmanager kcolorchooser ekcharselect kfind -y`

### Packages required for configuring Japanese input on KDE

$ `sudo dnf install kcm-fcitx5 fcitx-qt5 fcitx5-qt-libfcitx5qt5widgets fcitx5-mozc fcitx5-autostart fcitx5 fcitx-libs fcitx5-qt-module fcitx5-qt-libfcitx5qtdbus fcitx5-qt fcitx5-gtk fcitx5-gtk4 fcitx5-gtk3 fcitx5-gtk2 fcitx5-data`

After that add input method with Fcitx called Mozc. You might also want to enable global shortcut to switch input methods. I recommend using Super Key + Space key.

### Switch to SDDM

$ `sudo systemctl enable sddm.service -f`

### Switch to GDM

$ `sudo systemctl enable gdm.service -f`

### Switch Fedora Edition from vanilla (Gnome) to KDE

$ `sudo dnf swap fedora-release-identity-workstation fedora-release-identity-kde`

### Switch Fedora Edition from KDE to vanilla (Gnome)

$ `sudo dnf swap fedora-release-identity-kde fedora-release-identity-workstation`

### Disable ksshaskpass

$ `unset SSH_ASKPASS`

### List last installed packages

$ ` rpm -qa --last | head --lines=5`

Change `5` with the number of packages you want to list

## OpenSUSE

### Low resolution in OpenSUSE GUI installer
I am not sure why this happens, it used to work fine before on same hardware. There are two possibilities, I replaced my screen and vendor probably installed a knockoff version on my machine, or Novell broke the installer (I am using tumbleweed edition). This probably can be worked around by passing resolution parameters in grub before the installer starts. See more, https://linuxhint.com/set_screen_resolution_linux_kernel_boot/

### Fix OpenSUSE not giving enough time to enter LUKS password:

As soon you see your system booting after grub press "esc" button to see console, look out for password input prompt, as soon as you see it quickly press enter button and enter your password and press enter again. As soon as you can boot into your system run the following:

$ `sudo nano /etc/crypttab`

You will see a line something like

`cr-auto-1  UUID=43a6aea6-94e5-47fe-b524-34fd1e969ee5`

Add timeout option to the line, which will result in something like the following line

`cr-auto-1  UUID=43a6aea6-94e5-47fe-b524-34fd1e258ee5 timeout=180`

`180` means seconds the system will wait for you to input your LUKS password

### Disable autologin

YaST > Security and Users > User and Group Management
Expert Options > Login Settings
Uncheck Auto Login

### Manually modify mirrors

Visit https://mirrors.opensuse.org/ and choose a mirror, select either HTTP or FTP as a base URL. Copy it and visit it,

YaST > Software > Software Repositories

As you can see in GUI, you have to make repo URLs using the base URL of the mirror you just copied, so create the repo URLs with your base URL and also verify by visiting them using a browser. Then add them using the "Add" button. You may also need to accept terms and conditions when adding a repo.
The default repo priority is 99, to make zypper prefer your selected repos you need to set their priority lower than 99. Trying zypper dup in terminal can also hint to you if your repos are hosting older software if zypper wants you to downgrade your packages. In that case, you have two options, you can either reselect another mirror or just keep the priority of the mirror the same as the default ones i.e. 99.

Below is an example of creating repo URLs from a base mirror URL http://ftp.kddilabs.jp/Linux/packages/opensuse/ :

JP-Non-Oss http://ftp-srv2.kddilabs.jp/Linux/packages/opensuse/tumbleweed/repo/non-oss

JP-Oss http://ftp-srv2.kddilabs.jp/Linux/packages/opensuse/tumbleweed/repo/oss

JP-Source http://ftp-srv2.kddilabs.jp/Linux/packages/opensuse/source/tumbleweed/repo/oss/

JP-Update http://ftp-srv2.kddilabs.jp/Linux/packages/opensuse/update/openSUSE-current/

### Enable ZRAM

$ `sudo zypper in systemd-zram-service && sudo zramswapon`

### Change default session for auto login if SDDM is enabled instead of GDM

SDDM Settings > Behaviour

### Install indian fonts

$ `sudo zypper in indic-fonts`

### Installing plugins for playing movies and music + Firefox media playback

https://en.opensuse.org/SDB:Installing_codecs_from_Packman_repositories

### Relabel files for SELinux if Desktop Environment does not let you login (This also happens when you reuse /home when install Fedora KDE Spin)

Add `autorelabel` option to your cmdline by temporarily editing grub entry. You can do that when you are on grub screen. Just highlight a kernel and press E button and add it to cmdline and save it by pressing `Ctrl + X`.

Usually OpenSUSE, Fedora and RHEL are known to do it automatically.
