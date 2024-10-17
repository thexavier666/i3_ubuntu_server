# Installing i3 in Ubuntu Server without a Desktop Environment

## Assumptions

1. You have a basic Ubuntu installation, possibly Ubuntu Server 18.04
2. You have proper network connectivity

## Installation

### Install `x-server`

```
$ sudo apt install xorg xinit
```

### Install apps related to compiling and making `i3`

```
$ sudo apt install git make gcc autoconf
```

### Install all dependancies for `i3`

```
$ sudo apt install libpango1.0-dev libyajl-dev libstartup-notification0-dev libev-dev libtool libxkbcommon-dev libxkbcommon-x11-dev libxcb1-dev libxcb-randr0-dev libxcb-util0-dev libxcb-icccm4-dev libxcb-keysyms1-dev libxcb-cursor-dev libxcb-xinerama0-dev libxcb-xkb-dev libxcb-shape0-dev libxcb-xrm-dev xutils-dev
```

### Installing i3

[Source](https://github.com/pasiegel/i3-gaps-install-ubuntu)

You can install `i3` using your package manager, but I prefer to install from source. 

There are 3 popular versions of `i3` that I know of

1. The original [`i3`](https://github.com/i3/i3)
2. [`i3-gaps`](https://github.com/Airblader/i3) by [Airblader](https://github.com/Airblader) which has support for gaps between windows
3. [`i3-gaps`](https://github.com/resloved/i3) by [resolved](https://github.com/resloved) which has support for rounded corner for windows

I prefer installing [`i3-gaps`](https://github.com/resloved/i3) by resolved. However, the installation procedure is the same for all variants.

```
$ git clone https://github.com/resloved/i3
$ cd i3
$ autoreconf --force --install
$ rm -rf build
$ mkdir build
$ cd build
$ ../configure --prefix=/usr --sysconfdir=/etc
$ make
$ sudo make install
```

At this point, your `i3` installation is complete

Note : In case you get a dependancy error, install the required packages and repeat the above step from scratch.

## Enabling i3 to run after login

### Create `~/.xinitrc` and edit it as follows

```
#!/bin/sh
# /etc/X11/xinit/xinitrc
# global xinitrc file, used by all X sessions started by xinit (startx)
exec /usr/bin/i3
```

### Run `startx`

1. Login to your shell
2. Run `startx`

Your `i3` WM should start up

---

## Enabling `i3` for Remote Desktop Protocol

[Source](https://linuxize.com/post/how-to-install-xrdp-on-ubuntu-18-04/)

### Install `xrdp` server

```
$ sudo apt install xrdp
```

### Enabling `i3` upon login using `rdp` for single user

1. Edit the file `~/.xsessions` or create a new file
2. Add the following lines to the file

    ```
    #!/bin/sh
    exec /usr/bin/i3
    ```
3. Restart `xrdp` service

    ```
    $ sudo systemctl restart xrdp.service
    ```

Now you can connect to this machine into `i3` using any RDP client.

### Enabling `i3` upon login using `rdp` for all users

Keep in mind that `i3` RDP login has been mapped to your account only.
If you want to set `i3` as the default RDP login for all, edit the file `/etc/xrdp/startwm.sh` and do steps 2 and 3 as mentioned in the previous section.
