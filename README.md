# config

Configuration of tools I use:
* [lubuntu](#lubuntu)
* [guake](#guake)
* [micro](#micro)
* [.bashrc](#bashrc)
* [vlc](#vlc)

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

On noise from speakers:
https://askubuntu.com/a/1230834

```
echo 0 | sudo tee /sys/module/snd_hda_intel/parameters/power_save
echo "options snd_hda_intel power_save=0" \
    | sudo tee -a /etc/modprobe.d/audio_disable_powersave.conf
```

## guake

http://guake-project.org/

Install:
```
sudo apt install guake dconf-cli
guake --restore-preferences=guake.dconf
lxqt-config-session
# Autostart - Add - guake
# App Menu - System Tools - Guake Terminal
```

Usage:
* Press `Pause` button, again, again
* On python traceback, Ctrl+Click file/line to open it in `micro` editor below

## micro

https://micro-editor.github.io/

Install:
```
sudo snap install micro --classic
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

External clipboard issue:
* Exit micro
* Try `sudo apt install xclip`
* If it still fails, `sudo apt remove xclip && sudo apt install xsel`
* Avoid copy-paste of big chunk of text from micro to GUI: it may get freezed

## .bashrc

```
e ~/.bashrc
# Append to the end:
alias e=micro
alias gd='git diff --color-words'
alias gl='git log --decorate=full --graph'
alias o=xdg-open
export CDPATH=$REDACTED/cdpath
export EDITOR=micro
```

## vlc

https://www.videolan.org/vlc/

Configure:
```
cp vlc/* ~/.config/vlc/
```

Usage:
* Drag mouse left-right for pause/play
