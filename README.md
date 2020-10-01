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

External clipboard issue:
* Exit micro
* Try `sudo apt install xclip`
* If it still fails, `sudo apt remove xclip && sudo apt install xsel`
* Avoid copy-paste of big chunk of text from micro to GUI: it may get freezed
