# neovim-cygport
Contains cygport files to build [neovim](https://neovim.io/).  Resulting product not ~~heavily~~ tested.

## Building
Use [cygport](https://github.com/cygwinports/cygport) to build these.

### Build order
libtermkey has an optional dependency on unibilium, and neovim depends on all the other packages (though lua-mpack is a build time dependency).

1. libuv libvterm lua-mpack msgpack unibilium
1. libtermkey
1. neovim

## Installing
To use generated packages with Cygwin's installer, move the dist/<package> dirs into a directory called x86\_64 (or x86 if building 32-bit packagess), use [mksetupini](https://cygwin.com/git/?p=cygwin-apps/calm.git;a=blob;f=calm/mksetupini.py;h=e7337fe18e77003be43559f1c8a686882d11ded1;hb=bcf17529eb9ca8ae85f1df7d0f39ea9a277fd378) from [calm](https://cygwin.com/git/?p=cygwin-apps/calm.git) to generate a setup.ini file, then run setup and specify the parent directory as a site.  The commands I use (including signing, which is not necessary) are:

    mksetupini --arch x86_64 --inifile x86_64/setup.ini --releasearea=. --okmissing required-package; and gpg --local-user=cygwin@host --detach-sign x86_64/setup.ini
    ~/Downloads/setup-x86_64.exe --no-shortcuts --root=(cygpath -w /) --site=<your cygwin mirror> --site=(cygpath -w (pwd)) --pubkey=(cygpath -w ~/cygwin-dsa.key)
