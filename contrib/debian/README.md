
Debian
====================
This directory contains files used to package Crowncoind/Crowncoin-qt
for Debian-based Linux systems. If you compile Crowncoind/Crowncoin-qt yourself, there are some useful files here.

## Crowncoin: URI support ##


Crowncoin-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install Crowncoin-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your Crowncoin-qt binary to `/usr/bin`
and the `../../share/pixmaps/Crowncoin128.png` to `/usr/share/pixmaps`

Crowncoin-qt.protocol (KDE)

