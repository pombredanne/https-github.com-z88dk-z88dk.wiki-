# Snap usage

Snaps for z88dk are now available on the Snap store. The version tracks the latest source code (and is built on every change). To install:

    sudo snap install --edge z88dk

All commands within a snap are namespaced which means that by default to launch `zcc` you have to enter the command `z88dk.zcc`. To remove the namespaced prefix, you need to add some aliases:

    sudo snap alias zcc z88dk.zcc
    sudo snap alias z88dk-z80asm z88dk.z88dk-z80asm
    sudo snap alias z88dk-ticks z88dk.z88dk-ticks
    sudo snap alias z88dk-dis z88dk.z88dk-dis
    sudo snap alias z88dk-appmake z88dk.z88dk-appmake

And so on, for all the commands that you want to use.


