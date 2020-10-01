# config

Configuration of tools I use

## install

```
sudo apt-get install git
git clone https://github.com/denis-ryzhkov/config.git
cd config
```

## micro

https://micro-editor.github.io/ by [@zyedidia](https://github.com/zyedidia)

```
sudo snap install micro --classic
micro -plugin install bounce manipulator
sudo apt-get install xsel
cp micro/* ~/.config/micro
echo 'alias e=micro' >> ~/.bashrc
exit
```

Usage:
```
e README.md
# Ctrl+E
# help
# Ctrl+Q
```

Known issues:
* xsel freezes?
    * Copy a lot of text in micro, paste in GUI, e.g. Chrome
    * Result: Chrome is freezed
    * Workaround: `killall chrome`, copy-paste smaller chunks
