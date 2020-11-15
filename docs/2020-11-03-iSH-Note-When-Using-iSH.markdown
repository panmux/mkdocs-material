

# Notes When Using iSH on iPadOS

After using iSH on iPad for quite a while, I have some notes.

# iSH is now available on App Store

Just in case you are still using iSH through TestFight, it is to inform you that it is now available on App Store.


# Trick to run iSH background

+ Note: 
  - Using `cat /dev/location` and turned on location service will consume more battery.
  
  - You may not need this trick at all, you can simply `Split View` iSH over Safari to achieve the same effect.

  - You can also use `phpwin` app on iPad for opening offline website.


However, if you really need to run it background, for example with python module `http.server`, try this:

+ Create and chmod executable file `runBackground.sh`

+ Add these:

```bash
cat /dev/location > /dev/null &
python3 -m http.server ~/www/

```

+ Now turn on your device Location service in the device setting, and allow iSH use your location while running.

+ Run `sh runBacground.sh`

+ Now open Safari and access: `http://localhost:8000/`
Change the port `8080` accordingly to your case. You should stay with Safari while iSH is still running.



# php and composer

To install php and composer on iSH:



```bash
apk add php7 php7-phar php7-mbstring php7-json php7-openssl

```


Create and chmod 777 `installComposer.sh` with the below bash script:


``` bash
# This snippet is from: 
https://getcomposer.org/doc/faqs/how-to-install-composer-programmatically.md

#!/bin/sh

EXPECTED_CHECKSUM="$(wget -q -O - https://composer.github.io/installer.sig)"
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
ACTUAL_CHECKSUM="$(php -r "echo hash_file('sha384', 'composer-setup.php');")"

if [ "$EXPECTED_CHECKSUM" != "$ACTUAL_CHECKSUM" ]
then
    >&2 echo 'ERROR: Invalid installer checksum'
    rm composer-setup.php
    exit 1
fi

php composer-setup.php --quiet
RESULT=$?
rm composer-setup.php
exit $RESULT

```

To have the command composer available globally, we move composer.phar to:

```bash
mv composer.phar /usr/local/bin/composer
```


# webserver php with apache

An easy ways to install webserver php with apache is via this auto script:

``` bash
git clone https://github.com/timtim8461/ish-webserver-setup.git
cd ish-webserver-setup/
chmod 777 *.sh
chmod +x setup.sh
chmod +x start.sh 
./setup.sh
```

# Find php.ini and install SQLite3 extension

Enable SQLite3 extension. SQLite3 is shipped with php, so we just need to enable it in the php.ini.

However on iSH we may need to install it manually:


```bash
p:~# apk search sqlite3
p:~# apk add php7-sqlite3
```

We can find php.ini location with these commands:

+ The better way (faster) is:

```bash
php -i | grep php.ini
```

+ Or slower one:

```bash
p:~# cd /
p:/# find . -name 'php.ini'
./etc/php7/php.ini

```

And now we can edit it with nano:

```bash
p:/# nano /etc/php7/php.ini
```

In nano, press Ctrl + w and find sqlite3 and uncomment ; to enable it. We will get something:

```ini
;extension=sockets
extension=sqlite3
;extension=tidy
```

You may need to restart your server to get SQLite3 extension working.

Check again


```bash
p:~# php -i | grep php.ini

PHP Warning:  Module 'sqlite3' already loaded in Unknown on line 0
Configuration File (php.ini) Path => /etc/php7
Loaded Configuration File => /etc/php7/php.ini

```

# Java javac

Install

```java
apk search openjdk
apk add openjdk8

```

If running `java -version` yields error, try this command:



```bash
java -mx512m -version
```

You can now try running a jar file:

```bash
java -mx512m -jar test.jar

```

If running ` javac -version` yields error

```
javac -version
ash: javac: not found
```

Try to set these paths:

```
##### 

export JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk
export PATH="$JAVA_HOME/bin:${PATH}"
```

You can put these commands to:

`/etc/profile.d/profile.sh`

to set these vars automatically every time you start up the terminal.



+ If you have a newer version, try updating YOURversion in the above command, like `/usr/lib/jvm/YOURversion`

If things go smooth, you will get response something like:

```bash
javac -version
javac 1.8.0_252
```

To remove these above path

List env with `export` or `env`

```
env
export
```

```
export
unset JAVA_HOME
unset PATH
```


# pynvim

On iSH (iPad) If pip3 install pynvim yields errors (greenlet)



```include/python3.8/Python.h:11:10: fatal  error: limits.h: No such file or directory
    #include <limits.h>
                        ^
    compilation terminated.
    
```



Fix:
====


``` bash
apk add --no-cache gcc linux-headers musl-dev

```


