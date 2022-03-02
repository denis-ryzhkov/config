# config

Configuration of tools I use:
* [lubuntu](#lubuntu)
* [nvidia](#nvidia)
* [kitty](#kitty)
* [micro](#micro)
* [bash](#bash)
* [git](#git)
* [vlc](#vlc)
* [vsftpd](#vsftpd)
* [docker](#docker)

Install:
```
git clone https://github.com/denis-ryzhkov/config.git
cd config
```

## lubuntu

https://lubuntu.me/

Install:
* https://lubuntu.me/downloads/
* https://help.ubuntu.com/community/mkusb

### On touchpad touch-to-click fails

* `lxqt-config-input`
* Scroll to "Mouse and Touchpad"
* Select "Device: Touchpad"
* Enable "Tap to click"

### On WiFi shows networks but fails to connect

```
iw reg get  # See current country
iw reg set $CC  # Two-letter ISO country code
```

### On noise from speakers

* https://askubuntu.com/a/1230834

```
echo 0 | sudo tee /sys/module/snd_hda_intel/parameters/power_save
echo "options snd_hda_intel power_save=0" \
    | sudo tee -a /etc/modprobe.d/audio_disable_powersave.conf
```

### On keyring popup

* https://askubuntu.com/a/666664

```
sudo apt install libpam-gnome-keyring
echo 'password optional pam_gnome_keyring.so' | sudo tee /etc/pam.d/passwd
echo 'session optional pam_gnome_keyring.so auto_start' | sudo tee /etc/pam.d/login
```

## nvidia

Install:
```
sudo su
add-apt-repository ppa:graphics-drivers/ppa
ubuntu-drivers devices
# Note the recommended driver version
apt install nvidia-driver-470
reboot
```

## kitty

https://sw.kovidgoyal.net/kitty/

Install:
```
curl -L https://sw.kovidgoyal.net/kitty/installer.sh | bash
```

Configure:
```
cp kitty/dr.conf ~/.config/kitty/
echo -e '\ninclude dr.conf' >> ~/.config/kitty/kitty.conf

sudo visudo
# Append:
# Defaults env_keep += "TERM TERMINFO"

lxqt-config-globalkeyshortcuts
# Find "Control+Alt+T" - Modify - Command:
# /home/dr/.local/kitty.app/bin/kitty
```

Usage:
* Ctrl+Alt+T
* Defaults: https://sw.kovidgoyal.net/kitty/conf/
* Personal: [kitty/dr.conf](kitty/dr.conf)

## micro

https://micro-editor.github.io/

Install:
```
sudo snap install micro --classic
```

Configure:
```
micro -plugin install bounce manipulator
sudo !!
cp micro/* ~/.config/micro/
sudo cp micro/* /root/.config/micro/
echo -e '\nalias e=micro' >> ~/.bashrc
echo -e '\nalias e=micro' | sudo tee -a /root/.bashrc
exit
```

Usage:
```
e README.md
# Ctrl+E
# help
# Ctrl+Q
```

### On external clipboard issue

* Exit micro
* Try `sudo apt install xclip`
* If it still fails, `sudo apt remove xclip && sudo apt install xsel`
* Avoid copy-paste of big chunk of text from micro to GUI: it may get freezed

## bash

https://www.gnu.org/software/bash/

Configure:
```
e ~/.bashrc
# Append to the end:

alias cd='cd -P'
alias e=micro
alias gd='git diff --color-words'
alias gl='git log --decorate=full --graph'
alias ll='ls -alF --full-time'
alias o=xdg-open

function difl { colordiff -u "$1" "$2" | less -r; }
function difw { wdiff "$1" "$2" | colordiff | less -r; }

CDPATH=/home/dr/.cdpath  # cd .cdpath && ln -s /path/to/frequent/dir && cd dir
EDITOR=micro
VISUAL=$EDITOR
```

* `git commit` kept ignoring `micro` even with `GIT_EDITOR` var,
but `git config --global core.editor micro` solved the issue

## git

https://git-scm.com/

Configure:
```
cp git/.gitconfig ~/
e ~/.gitconfig
# Update `name` and `email`
```

## vlc

https://www.videolan.org/vlc/

Run and exit VLC to make sure it uses `~/.config/vlc`

Configure:
```
cp vlc/* ~/.config/vlc/
```

Usage:
* Drag mouse left-right for pause/play
* Drag mouse left or right to skip 10 seconds

## vsftpd

https://security.appspot.com/vsftpd.html

Why:
* "VLC - Playback - Renderer - TV" requires re-encoding most videos,
  TV remote control becomes useless, bad UX
* VLC for TV plays videos from removable media with best UX,
  but you need to insert and safe-eject it on both devices every time
* VLC for TV can play videos on FTP server:

Install:
```
sudo apt install vsftpd
```

Configure:
```
sudo su
e /etc/vsftpd.conf
    local_enable=YES
    chroot_local_user=YES
    xferlog_enable=NO

service vsftpd restart
useradd -rm pub
passwd pub
rm /home/pub/.*
exit

sudo chown $USER:pub /home/pub
cd ~/Downloads
ln -s /home/pub
mv $FILES pub/
```

Test:
```
ifconfig | grep 192.168
ftp 192.168.1.36
ls
```

* VLC for TV - Add server to Favorities - the server added - play videos!
* If VLC for TV fails to play some videos via FTP,
  then install "X-plore" app for TV, connect to the same FTP server,
  play videos, or copy them to TV's internal storage, and play them with VLC for TV

## docker

https://www.docker.com/

Cleanup:
```
docker system prune --all --volumes
```
