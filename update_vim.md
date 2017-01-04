# a guide for update vim

##(1) Update gcc

```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update

    sudo apt-get upgrade
    sudo apt-get install gcc-4.8 g++-4.8
    sudo apt-get install gcc-4.9 g++-4.9
    sudo apt-get install gcc-5 g++-5

    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 48 \
    --slave /usr/bin/g++ g++ /usr/bin/g++-4.8 \
    --slave /usr/bin/gcc-ar gcc-ar /usr/bin/gcc-ar-4.8 \
    --slave /usr/bin/gcc-nm gcc-nm /usr/bin/gcc-nm-4.8 \
    --slave /usr/bin/gcc-ranlib gcc-ranlib /usr/bin/gcc-ranlib-4.8

    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 49 \
    --slave /usr/bin/g++ g++ /usr/bin/g++-4.9 \
    --slave /usr/bin/gcc-ar gcc-ar /usr/bin/gcc-ar-4.9 \
    --slave /usr/bin/gcc-nm gcc-nm /usr/bin/gcc-nm-4.9 \
    --slave /usr/bin/gcc-ranlib gcc-ranlib /usr/bin/gcc-ranlib-4.9

    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 53 \
    --slave /usr/bin/g++ g++ /usr/bin/g++-5 \
    --slave /usr/bin/gcc-ar gcc-ar /usr/bin/gcc-ar-5 \
    --slave /usr/bin/gcc-nm gcc-nm /usr/bin/gcc-nm-5 \
    --slave /usr/bin/gcc-ranlib gcc-ranlib /usr/bin/gcc-ranlib-5
```

choose gcc 4.9 as default version
```
    sudo update-alternatives --config gcc
```

## (2) build vim

you can refer to below links
>[buid vim from source](https://github.com/Valloric/YouCompleteMe/wiki/Building-Vim-from-source)

>[update vim](http://ram.kossboss.com/update-update-alternatives-of-vim-to-new-vim-statically-compiled-vim/)

### 1. install prerequisite libraries

```
    sudo apt-get install libncurses5-dev libgnome2-dev libgnomeui-dev \
        libgtk2.0-dev libatk1.0-dev libbonoboui2-dev \
        libcairo2-dev libx11-dev libxpm-dev libxt-dev python-dev \
        python3-dev ruby-dev lua5.1 lua5.1-dev libperl-dev git
```

### 2. build

```
    cd ~
    git clone https://github.com/vim/vim.git
    cd vim
    ./configure --with-features=huge \
                --enable-multibyte \
                --enable-rubyinterp=yes \
                --enable-pythoninterp=yes \
                --with-python-config-dir=/usr/lib/python2.7/config \
                --enable-perlinterp=yes \
                --enable-luainterp=yes \
                --with-compiledby='WenteWang <winter.seu@gmail.com>' \
                --enable-gui=gtk2 --enable-cscope --prefix=/usr
    make VIMRUNTIMEDIR=/usr/share/vim/vim80
    cp /usr/bin/vim /usr/bin/vim.old
    make install
```

### 3. use update-alternatives to select built vim

```
# this changes every alternative seen in "update-alternatives --get-selections" with a specific target to a new target.
# before pasting: change OLD_TARGET and NEW_TARGET and NEW_PRIO to match your needs
    (OLD_TARGET="/usr/bin/vim.old"; NEW_TARGET="/usr/bin/vim80"; NEW_PRIO=50; echo "*** BEFORE ***"; update-alternatives --get-selections; echo "*********"; IFS=$'\n'; for ALTERNATIVE in `update-alternatives --get-selections | grep ${OLD_TARGET} | awk '{print $1}'`; do update-alternatives --verbose --install `which ${ALTERNATIVE}` ${ALTERNATIVE} ${NEW_TARGET} ${NEW_PRIO}; update-alternatives --set ${ALTERNATIVE} ${NEW_TARGET}; done; echo "*** AFTER ***"; update-alternatives --get-selections)
# below commands is for remove the alternatives
    (OLD_TARGET="/usr/bin/vim"; echo "*** BEFORE ***"; echo "*********"; IFS=$'\n'; for ALTERNATIVE in `update-alternatives --get-selections | grep ${NEW_TARGET} | awk '{print $1}'`; do echo ${ALTERNATIVE}; update-alternatives --verbose --remove ${ALTERNATIVE} ${OLD_TARGET} ; done; echo "*** AFTER ***")
```
