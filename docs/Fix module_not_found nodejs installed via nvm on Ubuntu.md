# Fix module_not_found nodejs installed via nvm on Ubuntu 20.04.md

So you have installed a global module successfully with `npm i -g <package>`.

However when you require it via `require('<package>')`, it would complain with error code: `'MODULE_NOT_FOUND'`.

Here is a fix that worked for me.

+ Check whether it is true that the required package is installed globally.

`npm list -g --depth=0`

This command will show which packages are installed globally.


+ Now check these which paths:

```

d@d1:~/dev/tipitakaAppEdit/code-edit$ which npm
/home/user/.nvm/versions/node/v12.18.3/bin/npm
d@d1:~/dev/tipitakaAppEdit/code-edit$ which node
/home/user/.nvm/versions/node/v12.18.3/bin/node
d@d1:~/dev/tipitakaAppEdit/code-edit$ echo $NODE_PATH
//empty line
```

The echo `$NODE_PATH `does show an empty line on my terminal. 

Append the `node_modules` to the path returned by `which node`  as follow:

from

 `/home/user/.nvm/versions/node/v12.18.3/bin/node` 

to

`/home/user/.nvm/versions/node/v12.18.3/lib/node_modules`


+ Now from your terminal, you can run

`export NODE_PATH=/home/user/.nvm/versions/node/v12.18.3/lib/node_modules`

**Bonus**:

> To apply this every time you open your terminal, you can create and add the above export command to
>
> `/etc/profile.d/profile.sh`
>
> Or append the export command to
>
> `~/.bashrc`
>
> Refresh/reload:
>
> `source ~/.bashrc`
>
> Check with one is working for you. Type
>
> `env`



