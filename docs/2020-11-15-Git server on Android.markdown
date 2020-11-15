# Set up a Git server on Android

I used an open source git server for Android app.

https://github.com/panmux/Andoird-Gitrepo


I used buddy.works to build this apk and signed with my own keystore. 

The keystore password key is your nama two times.

## Note:
On Gitrepo app
+ Create user
+ Add public key
+ Create Repositories
+ Add user to respositories

** You can not clone this respository now. You need to git init first.

## Git init

+ Enable hotspot and 'Start' ssh server from Gitrepo

+ `cd` to the the newly created git repo with Termux.


```bash

cd local
git init
nano Readme.md
git add .
git commit -m 'initial commit'
# panmux is an example user with full pull push permission for 'local' git project
git remote add origin ssh://panmux@192.168.43.1:2222/local.git
git push origin master

```

## Now you can ssh clone this local.git from other devices

```bash

git clone ssh://panmux@192.168.43.1:2222/local


```




## The server seems to work on iPad,

On other devide, `git push` successfully  but cannot changes reflected on Android.

However, if I clone that repository to another dir, it show my latest commit files.





