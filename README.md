# config

Configuration of tools I use:
* [lubuntu](#lubuntu)
* [macos](#macos)
* [kitty](#kitty)
* [micro](#micro)
* [bash](#bash)
* [git](#git)
* [vlc](#vlc)
* [smplayer](#smplayer)
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
* Open https://lubuntu.me/downloads/
* Download the magnet link
* Open it with a BitTorrent client like the default Transmission
* Install `mkusb` as in https://help.ubuntu.com/community/mkusb
* Prepare, but don't insert the USB drive
* Wait until ISO file gets downloaded
* Run `mkusb`, follow the instructions, select:
    * `p` - Plug
    * `l` - Live drive with 'usbdata' partition
    * `n` - NTFS
    * `n` - No other filesystems

* Boot the target computer from this USB drive and install lubuntu

### Tap to click

* `lxqt-config-input`
* Scroll to "Mouse and Touchpad"
* Select "Device: Touchpad"
* Enable "Tap to click"

### On WiFi shows networks but fails to connect

```
iw reg get  # See current country
iw reg set $CC  # Two-letter ISO country code
```

### On brightness not adjustable

```
sudo chmod u+s /usr/bin/lxqt-backlight_backend
lxqt-config-globalkeyshortcuts
# XF86MonBrightnessDown - Command: lxqt-backlight_backend --dec
# XF86MonBrightnessUp - Command: lxqt-backlight_backend --inc
```

### On `ibus` shipped with `zoom` breaks keyboard layouts

```
ibus exit
echo 'run_im none' > ~/.xinputrc
```

### On noise from speakers

* https://askubuntu.com/a/1230834

```
echo 0 | sudo tee /sys/module/snd_hda_intel/parameters/power_save
echo "options snd_hda_intel power_save=0" \
    | sudo tee -a /etc/modprobe.d/audio_disable_powersave.conf
```

### nvidia driver

```
sudo su
add-apt-repository ppa:graphics-drivers/ppa
ubuntu-drivers devices
# Note the recommended driver version
apt install nvidia-driver-470
reboot
```

## macos

### On Logitech mouse wheel scroll issues

Install and configure https://www.logitech.com/en-us/software/logi-options-plus.html

### karabiner

https://karabiner-elements.pqrs.org/

Install from the link above.

Configure:
```
cp karabiner/dr.json ~/.config/karabiner/assets/complex_modifications/
```

Open "Karabiner-Elements" - Complex Modifications - Add rule - Personal - Enable All

Docs:
[files](https://karabiner-elements.pqrs.org/docs/json/root-data-structure/#custom-json-file-in-configkarabinerassetscomplex_modifications),
[manipulators](https://karabiner-elements.pqrs.org/docs/json/complex-modifications-manipulator-definition/)

### homebrew

https://brew.sh/

Install:
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
eval "$(/opt/homebrew/bin/brew shellenv)"
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.bashrc
echo 'source ~/.bashrc' >> ~/.profile
brew analytics off
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
echo 'include dr.conf' >> ~/.config/kitty/kitty.conf

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

Install for Linux:
```
cd /tmp
curl https://getmic.ro | bash
sudo mv micro /usr/local/bin/
cd /usr/local/bin/
curl https://getmic.ro/r | sudo bash
```

Install for macOS:
```
brew install micro
```

Configure:
```
micro -plugin install bounce manipulator
sudo !!
cp micro/* ~/.config/micro/
sudo cp micro/* /root/.config/micro/
echo 'alias e=micro' >> ~/.bashrc
echo 'alias e=micro' | sudo tee -a /root/.bashrc
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

Install new version - for macOS only:
```
brew install bash
echo /opt/homebrew/bin/bash | sudo tee -a /etc/shells
chpass -s /opt/homebrew/bin/bash

brew install bash-completion
. /opt/homebrew/etc/profile.d/bash_completion.sh
echo '. /opt/homebrew/etc/profile.d/bash_completion.sh' >> ~/.bashrc
```

Configure:
```
e ~/.bashrc
# Append to the end:

alias cd='cd -P'
alias cp='rsync -ah --progress'
alias difl='colordiff -u'
alias e=micro
alias gd='git diff --color-words'
alias gl='git log --decorate=full --graph'

alias l='ls -a1F'
alias ll='ls -alF --time-style=long-iso'
# For macOS: alias ll='ls -alFD "%F %T %Z"'

alias o=xdg-open
# For macOS: alias o=open

CDPATH=$HOME/.cdpath  # cd .cdpath && ln -s /path/to/frequent/dir && cd dir
EDITOR=micro
PS1='\[\033[35m\]\u@\h:\w\$\[\033[0m\] '
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

### On audio not playing after pause/resume

* Tools - Preferences - Audio:
    * Output: ALSA
    * Device: HD-Audio Generic, MODEL Analog Hardware device with all software conversions

## smplayer

https://www.smplayer.info/

Why:
* VLC failed to load an audio track from external file, SMPlayer succeeded.

Install:
```
sudo add-apt-repository ppa:rvm/smplayer
sudo apt update
sudo apt install smplayer smplayer-themes
```

Configure:
* Options - Preferences
    * Interface
        * Interface - GUI: Mpc GUI
        * Floating control - [v] Show only when moving the mouse to the bottom of the screen
    * Keyboard and mouse
        * Mouse
            * Left click: Play / Pause
            * Right click: Change function of wheel
            * Drag function: Seek (horizontal) and volume (vertical)
        * Mouse wheel functions: Media seeking, Volume control

## vsftpd

https://security.appspot.com/vsftpd.html

Why:
* "VLC - Playback - Renderer - TV" requires re-encoding most videos,
  TV remote control becomes useless, bad UX
* VLC for TV plays videos from removable media with best UX,
  but you need to insert and safe-eject it on both devices every time
* VLC for TV can play videos on FTP server.
* FTP server is also useful to share files inside of WiFi LAN.

Install:
```
sudo apt install vsftpd
```

Configure:
```
sudo su
e /etc/vsftpd.conf
    local_enable=YES
    write_enable=YES
    chroot_local_user=YES
    xferlog_enable=NO

service vsftpd restart
useradd -rm pub
passwd pub
rm /home/pub/.*
mkdir /home/pub/write
exit

sudo chown -R $USER:pub /home/pub
cd ~/Downloads
ln -s /home/pub
mv $FILES pub/
# These new files will be owned by `$USER:$USER`, readable but not writable for `pub`,
# while `write` dir is still writable for `pub`.
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
