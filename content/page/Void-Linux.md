---
title: Void-Linux | Personal Pitfalls
description: "Problems I ran into running Void Linux, each with the fix that worked: ACPI errors and GRUB settings at boot, a dead touchpad and a missing GLX extension, Go downloads failing behind a bad DNS server and keeping a custom resolv.conf, Fcitx5 for Chinese input, a login manager that works, and a system clock that keeps drifting. Sources are listed at the end."
tags:
  - void
  - devlog
  - linux
date: 2022-12-03
lastMod: 2026-10-05
cover:
  image: https://img.charliie.dev/void.png
---

Notes from running Void Linux day to day. Each section states the problem, then what fixed it;
sources are collected at the bottom.

## Boot and GRUB

All three fixes below are edits to `/etc/default/grub`; apply them with `sudo update-grub` and
reboot.

### ACPI BIOS errors printed at boot

**Problem.** The boot log fills with `ACPI BIOS Error` lines just before the Void welcome
message, although nothing on the system misbehaves.

**Fix.** These come from bugs in the motherboard's ACPI tables, and the kernel has printed them by
default since around 2018; they are harmless.[^acpi-mint][^acpi-reddit]

1. Update the BIOS if the vendor ships a fix. That is the only real cure.
2. Otherwise hide them by lowering the kernel log level, from `loglevel=4` to `loglevel=0`:

   ```sh
   GRUB_CMDLINE_LINUX_DEFAULT="loglevel=0"
   ```

`acpi=off` also silences them, but it disables ACPI entirely (no power-off at halt, no power
saving), so it is not worth it for cosmetic errors.

### GRUB forgets the entry I booted last

**Problem.** GRUB always starts the first entry, even after picking another kernel or OS.

**Fix.** Let GRUB remember the last choice:[^grub-saved]

```sh
GRUB_DEFAULT=saved
GRUB_SAVEDEFAULT=true
```

### Other operating systems missing from the menu

**Problem.** A second OS on another partition does not show up in the GRUB menu.

**Fix.** GRUB 2.06 and later skip `os-prober` unless it is enabled explicitly:[^grub-osprober]

```sh
GRUB_DISABLE_OS_PROBER=false
```

## Hardware

### Touchpad not working on an Acer Aspire 4830

**Problem.** The touchpad on the Acer Aspire 4830 series is dead after installation.

**Fix.** Add `i8042.nomux=1` to the kernel command line, which stops the kernel from putting the
PS/2 controller into multiplexing mode that this touchpad does not handle:[^i8042]

```sh
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=0 i8042.nomux=1"
```

### `extension "GLX" missing` with an NVIDIA card

**Problem.** OpenGL programs fail with:

```text
Xlib:  extension "GLX" missing on display ":0".
```

**Fix.** Install the firmware package and switch to the open-source **nouveau** driver, which
fixed it for me:[^glx]

```sh
sudo xbps-install -S linux-firmware
```

## Network and DNS

### `go install` fails with `connection refused`

**Problem.** Fetching Go modules fails:

```text
dial tcp 142.251.42.241:443: connect: connection refused
```

**Fix.** Either point Go at a reachable module proxy:

```sh
go env -w GOPROXY=https://goproxy.cn
```

or stop using the router's DNS server: in my case removing `nameserver 192.168.0.1` from
`/etc/resolv.conf` was enough, which led to the next fix.

### Use Cloudflare DNS and keep `resolv.conf` from being overwritten

**Problem.** A hand-edited `/etc/resolv.conf` is replaced by the DHCP client on every reboot or
lease renewal.

**Fix.** Write the nameservers, then make the file immutable so nothing can rewrite it:[^resolv]

```sh
# /etc/resolv.conf
nameserver 1.1.1.1
nameserver 1.0.0.1
```

```sh
sudo chattr +i /etc/resolv.conf   # undo with: sudo chattr -i /etc/resolv.conf
```

The cleaner alternative is to tell the DHCP client to leave the file alone: `nohook resolv.conf`
in `/etc/dhcpcd.conf` for dhcpcd (Void's default), or a `make_resolv_conf` hook or a
`supersede domain-name-servers` line for dhclient, as the nixCraft article describes.

## Desktop

### Chinese input with Fcitx5

**Problem.** No way to type Chinese out of the box, and on a bare window manager the input method
does not start.

**Fix.** Install fonts, Fcitx5 with the Chewing engine and its toolkit modules:

```sh
sudo xbps-install -S fonts-droid-ttf wqy-microhei \
    fcitx5 fcitx5-configtool fcitx5-chewing fcitx5-chewing-icons \
    fcitx5-gtk fcitx5-gtk+2 fcitx5-gtk+3 fcitx5-gtk4 fcitx5-qt fcitx5-qt5 fcitx5-qt6
```

Start it from `~/.xinitrc`. If the desktop or window manager does not start a D-Bus session
itself, launch it through `dbus-launch`, or Fcitx5 cannot talk to applications:

```sh
# ~/.xinitrc
fcitx5 -d &
exec dbus-launch /usr/bin/herbstluftwm --locked
```

### Login manager

**Problem.** `ly` is buggy on Void.

**Fix.** Use `greetd` with the `tuigreet` greeter instead.

## System clock is wrong

**Problem.** The clock is off after boot even with the right time zone in `/etc/rc.conf`.

**Fix.** Work through the usual causes:[^void-time][^clock-reddit]

1. Run an NTP client. `chrony` syncs quickly and also writes the result back to the hardware clock
   by default; `openntpd` and `isc-ntpd` (package `ntp`) work too but sync more slowly.
2. If you dual boot with Windows, Windows keeps the hardware clock in local time while Linux
   assumes UTC. Store it as local time on the Linux side too:

   ```sh
   sudo hwclock --systohc --localtime
   ```

3. If it is still wrong, the BIOS clock itself (the RTC) is wrong. Fix it in the BIOS setup.

## Packages I install

Build tools and development headers I end up needing on a fresh install:

```sh
sudo xbps-install -S unzip wget git xz automake libtool autoconf cmake xtools man \
    libmagick libmagick-devel libmagic file-devel openssl-devel fontconfig-devel freetype-devel \
    harfbuzz harfbuzz-devel libevent-devel ncurses-devel libxcb-devel xclip \
    python3-devel ruby-devel lua-devel sqlite sqlite-devel
```

[^acpi-mint]: [\[Solved\] ACPI errors during boot](https://forums.linuxmint.com/viewtopic.php?p=2211434)
[^acpi-reddit]: [ACPI BIOS Error on startup](https://www.reddit.com/r/voidlinux/comments/v7qquj/acpi_bios_error_on_startup/)
[^grub-saved]: [Set "older" kernel as default grub entry](https://askubuntu.com/a/1000735)
[^grub-osprober]: [Configuration parameters - GRUB_DISABLE_OS_PROBER](https://hugh712.gitbooks.io/grub/content/configuration-parameters.html#GRUB_DISABLE_OS_PROBER)
[^i8042]: [What does the 'i8042.nomux=1' kernel option do during booting of Ubuntu?](https://unix.stackexchange.com/questions/28736/what-does-the-i8042-nomux-1-kernel-option-do-during-booting-of-ubuntu)
[^glx]: [Xlib: extension "GLX" missing on display ":1" - comment](https://github.com/servo/servo/issues/22908#issuecomment-465462090)
[^resolv]: [Linux Make Sure /etc/resolv.conf Never Get Updated By DHCP Client](https://www.cyberciti.biz/faq/dhclient-etcresolvconf-hooks/)
[^void-time]: [Date and Time](https://docs.voidlinux.org/config/date-time.html)
[^clock-reddit]: [Problems with time... again - comment](https://www.reddit.com/r/voidlinux/comments/km33pr/comment/ghcjf4l/?context=3)
