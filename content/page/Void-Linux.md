---
title: Void-Linux | Personal Pitfalls
description: "Notes from running Void Linux day to day: fixing the ACPI BIOS error at boot, saving GRUB options, an Acer Aspire 4830 touchpad that would not work and a Go install error. It also covers graphics drivers, xbps package tips, setting up Fcitx5 for Chinese input, switching DNS to Cloudflare, calibrating the system clock and choosing a display or login manager."
tags:
  - void
  - devlog
  - linux
date: 2022-12-03
lastMod: 2022-12-04
cover:
  image: https://img.charliie.dev/void.png
---

# Misc

## ACPI BIOS Error on startup

[\[Solved\] ACPI errors during boot](https://forums.linuxmint.com/viewtopic.php?p=2211434)

[ACPI BIOS Error on startup](https://www.reddit.com/r/voidlinux/comments/v7qquj/acpi_bios_error_on_startup/)

## Save grub option

[Set "older" kernel as default grub entry](https://askubuntu.com/a/1000735)

## Grub doc

[Configuration parameters - GRUB_DISABLE_OS_PROBER](https://hugh712.gitbooks.io/grub/content/configuration-parameters.html#GRUB_DISABLE_OS_PROBER)

## Acer Aspire 4830 series touchpad not working?

[What does the 'i8042.nomux=1' kernel option do during booting of Ubuntu?](https://unix.stackexchange.com/questions/28736/what-does-the-i8042-nomux-1-kernel-option-do-during-booting-of-ubuntu)

## golang install error

`dial tcp 142.251.42.241:443: connect: connection refused`

Change proxy might help

```
go env -w GOPROXY=https://goproxy.cn
```

or comment `nameserver 192.168.0.1` in `/etc/resolv.conf`.

---

# Graphical card

pkg: `linux-firmware`

`Xlib:  extension "GLX" missing on display ":0".`

[Xlib: extension "GLX" missing on display ":1" - comment](https://github.com/servo/servo/issues/22908#issuecomment-465462090)

switch to **nouveau** fix the problem

---

# pkg

unzip, wget, git, xz, automake, libtool, autoconf, libmagick, libmagick-devel, openssl-devel, cmake, fontconfig-devel, libevent-devel, ncurses-devel, harfbuzz, libxcb-devel, xclip, freetype-devel, harfbuzz-devel, libmagic, file-devel, xtools, man, python3-devel, ruby-devel, lua-devel, sqlite, sqlite-devel

---

# Fcitx5

font pkg: fonts-droid-ttf, wqy-microhei

input method & engine pkg: fcitx5 fcitx5-configtool fcitx5-gtk fcitx5-chewing fcitx5-chewing-icons `fcitx5-gtk` `fcitx5-gtk+2` `fcitx5-gtk+3` `fcitx5-gtk4` `fcitx5-qt` `fcitx5-qt5` `fcitx5-qt6`

In `~/.xinitrc`

```sh
fcitx5 -d &
```

In case your DE/WM doesn't auto-start **dbus**, add `dbus-launch` in `~/.xinitrc`

```sh
exec dbus-launch /usr/bin/herbstluftwm --locked
```

---

# Add CloudFlare DNS

In `/etc/resolv.conf`

```sh
;
; /etc/resolv.conf file for dnsmaster (sirius)
;
domain             doc.com
nameserver         1.1.1.1
nameserver         1.0.0.1
```

System will regenerate `/etc/resolv.conf` after reboot. So we need to prevent it.

`chattr +i /etc/resolv.conf`

[Linux Make Sure /etc/resolv.conf Never Get Updated By DHCP Client](https://www.cyberciti.biz/faq/dhclient-etcresolvconf-hooks/)

---

# Calibrate system clock

[Date and Time](https://docs.voidlinux.org/config/date-time.html)

pkg: ntp

[Problems with time... again - comment](https://www.reddit.com/r/voidlinux/comments/km33pr/comment/ghcjf4l/?context=3)

`sudo hwclock --systohc --localtime`

Sometimes it's the BIOS' clock not working properly, go the BIOS to fix it.

---

# Display/Login Manager

`ly` have some bug with void, use `greetd + tuigreet` instead
