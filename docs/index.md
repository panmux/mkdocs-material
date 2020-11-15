# 2020-11-03 Notes - When Using Termux On Android

[Termux](https://termux.com) on Android partly brings the power of a linux pc to Android tablets. With Termux, we can run python, php, nodejs, git, etc... and even Ubuntu (GUI via VNC) on one's Android powered devices.

Quote from its F-droid:

``` bash
Termux combines powerful terminal emulation with an extensive Linux package collection.
```

# Here are some of my notes when using this interesting app.

# Install

[Termux](https://termux.com)

I used the F-droid link to install Termux and its plugins.


# Do these things after the installation

This command will ask for storage permission for Termux:

`termux-setup-storage`

If you have root access, grant root access:

`su`

After that run exit command to log out root, and restart you Termux. Run it 2 times: 

`exit`

Then update & upgrade packages

```
pkg update

pkg upgrade
```

If you want to add more repositories:
`pkg install unstable-repo`

If you have root access:
`pkg install root-repo`


# Create some config files

### ~/.bash_profile

In your root dir, modify or create this dot file `~/.bash_profile`, you can add commands that you want Termux runs upon its starts up.

I use this config to change the default Termux dir to my work place dir, and add some alias commands.

For example, append these commands to `~/.bash_profile`:

```bash
echo -e 'Be midful'
cal;
cd ~/storage/shared/myWorkPlace/

alias gitt='git add .;git commit -m "from Termux";git push;'

```

+ Note: Android `/sdcard/` does not fully support chmod, (partally in `/data/media/`).

So you can not run `./file.sh` directly. You need to run `sh ./file.sh`. 


### ~/.nanorc
See .nanorc content in this respo

### ~/.vimrc
See .vimrc content in this respo

# Note when installing package


Sometimes when you install a `<package name>`, it may say that package is not available (mainly because of Internet connection), don't give up easily, you may try again with these commands:

```bash
pkg update
pkg search <package name>
pkg install <package name>

```


# Some useful packages


Install them one by one with this partten: `pkg install <package name> `.

Or you can put them all together in one command: `pkg install <packages name> <package name>`

```bash
git
python
nodejs
php-apache
nano
vim-python
wget
curl
rsync
ripgrep
poppler

```

All in one command:

   `pkg install git python nodejs php-apache nano vim-python wget curl rsync ripgrep poppler  `



# Notes & Guide

## openssh - generate and add ssh with openssh

`pkg install openssh`

+ Create ~/.ssh dir to store your ssh keys, normally openssh will auto create this dir for you.

`mkdir -p ~/.ssh`

+ Generate ssh key:

`ssh-keygen -t rsa -b 4096 -C "email@example.com" `

+ Choose default location to store this key, provide passphrase

`eval "$(ssh-agent -s)"`

+ Then immediately run
`ssh-add -k ~/.ssh/id_rsa`


## Git
`pkg install git`

### Set up
```bash
git config --global user.name "Panmux Android"

git config --global user.email "email@example.com"

git config --global core.editor nano
```

+ You can add ssh public key to your Github account: [https://github.com/settings/ssh/new](https://github.com/settings/ssh/new)

 

## rg RIPGREP
+ This grep tool is much faster than the ordinary grep. It supports Regex also.

`pkg install ripgrep`

This command will search for `keyword` in the entire current dir (including sub dirs).

It is really fast.

`rg keyword`


## pdftotxt (pkg install poppler)

The command will be `pdftotxt -enc UTF-8 inputfile.pdf`.

I use this to convert pdf files to txt which allow me to do index for full text search later. 

Sometimes, I also use `rg keyword` to quickly look for a keyword in many txt files inside the dir (and its subdirs also).

You can write a bash/shell script to do bulk convert:

```bash
# Credit: https://gist.github.com/tpaskhalis/214c3976ac08cb809d846e01135d9f5f
# Convert all pdf in FOLDER  (and its subdirs) to txt

FOLDER=/storage/ABC-SDCARD/Android/data/com.termux/files/_eBooks/

find "$FOLDER" -name '*.pdf' | while read i;
do
  echo "Processing $i"
  pdftotext -enc UTF-8 "$i"
done

```




## vim-python

+ On Termux I mainly use nano to edit files. Sometimes I also use `vim-python` instead of `vim`.

This package will enable `vim` with `python`, which allow us tp install many powerful python-powered plugins.

+ Should use plugin manager for vim, here I use `vim plug` (search it on Github).

Add plugins that you want to install to `~/.vimrc`.

+ Useful plugins: YouCompleteMe (for auto completion, search it on github, and should clone to the `~/.vim/plugged` after that cd to YouCompleteMe and install mannualy with this command: `python3 install.py --ts-completer`

(This plugin is too big to install via vim plugin.)

Note
`--ts-completer` is for Javascript family, you can add orther languages at the same time too.

In `~/.vimrc` I still keep the install command, for later update:

```bash
" Plugin manager vim-plug https://github.com/junegunn/vim-plug
  call plug#begin('~/.vim/plugged')

  Plug 'ycm-core/YouCompleteMe', { 'do': './install.py --ts-completer' }
" Should git clone and install manually with python3 install.py --ts-completer
" --ts-completer is more updated than the tern one

  call plug#end()
  
```

## Nodejs packages
Some useful packages. Install these with `npm i -g install <package name> `

```bash
express
http-server
live-server
epub-gen
prettier

```


## Python packages
Some useful packages. Install these with `pip3 install <package name> `

```bash
bs4
eng-to-ipa
```

## To install java on Termux

Install via this bash:


```bash
pkg install wget

wget https: //raw.githubusercontent.com/MasterDevX/java/master/installjava && bash installjava
```

When the installation is finished without complaining any errors, and if you get error when running `java -version`

Then try to install tsu (root permission required) then run again with
`pkg install tsu`

Then

`sudo java --version`

Or another way is, also with root permission, trying this method:

```bash

# from https://github.com/MasterDevX/Termux-Java/issues/7#issuecomment-653526860

su
export PATH=/data/data/com.termux/files/usr/bin:/data/data/com.termux/files/usr/bin/applets:$PATH
export LD_LIBRARY_PATH=/data/data/com.termux/files/usr/lib

```

* If this does not work, you can remove these exports by using the below unset command

* To list all env, run `env`, you should see a list of env

```bash
unset PATH
unset LD_LIBRARY_PATH
```

To auto set this env variables on startup of bash, place the command:

```
export PATH=/data/data/com.termux/files/usr/bin:/data/data/com.termux/files/usr/bin/applets:$PATH

export LD_LIBRARY_PATH=/data/data/com.termux/files/usr/lib

```

to` /etc/profile.d/myprofile.sh`

* To run a .jar file you can try this command:


```bash
sudo java -Xmx512M -Xms512M -jar filename.jar
```

> -Xmx512M and -Xms512M are to do some settings related to heap size. Google them to learn more.
