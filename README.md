# neovim-cygport
Contains cygport files to build [neovim](https://neovim.io/), currently 0.4.2, for Cygwin.  I've used neovim on Cygwin for two years.

[Could these packages be included in Cygwin someday?  Would they be, if submitted appropriately?](https://github.com/cascent/neovim-cygwin/issues/3)  I don't like mailing lists so I haven't tried.

## Binary packages
There is an ancient x86_64 build of neovim 0.2.2, from December 2017, available as a [tagged release](https://github.com/cascent/neovim-cygwin/releases).  I do not know if this will work with a recent Cygwin installation and do not recommend trying it.

## Building
Use [cygport](https://github.com/cygwinports/cygport) to build these.

If you'd like better instructions, or an easier process to use these cygport definitions, please create an issue.  If you've already made an improvement, please submit a pull request.
 
### Build order
luv depends on libuv, libtermkey has an optional dependency on unibilium, and neovim depends on all the other packages (though luajit-mpack and luajit-lpeg are build time dependencies).

1. libuv libvterm luajit-lpeg luajit-mpack msgpack-c unibilium
1. libtermkey luv
1. neovim

## Installing
To use generated packages with Cygwin's installer, move the dist/<package> dirs into a directory called x86\_64 (or x86 if building 32-bit packagess), use [mksetupini](https://cygwin.com/git/?p=cygwin-apps/calm.git;a=blob;f=calm/mksetupini.py;h=e7337fe18e77003be43559f1c8a686882d11ded1;hb=bcf17529eb9ca8ae85f1df7d0f39ea9a277fd378) from [calm](https://cygwin.com/git/?p=cygwin-apps/calm.git) to generate a setup.ini file, then run setup and specify the parent directory as a site.  The commands I use (including signing, which is not necessary) are:

    mksetupini --arch x86_64 --inifile x86_64/setup.ini --releasearea=. --okmissing required-package; and gpg --local-user=cygwin@host --detach-sign x86_64/setup.ini
    ~/Downloads/setup-x86_64.exe --no-shortcuts --root=(cygpath -w /) --site=<your cygwin mirror> --site=(cygpath -w (pwd)) --pubkey=(cygpath -w ~/cygwin-dsa.key)
