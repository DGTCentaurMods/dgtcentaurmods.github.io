---
layout: post
title:  "Network Share"
author: Ed
categories: [ Manual ]
image: assets/images/4.jpg
---
As of version 1.3.2 you the DGT Centaur Mods software provides a network share facility utilising webdav. This allows you to access the Centaur as a network drive!

## Accessing from Windows

Open File Explorer and type http://centaur.local into the address bar and hit enter  (if you named your board something other than centaur then use that!)

![File Explorer](/assets/images/webdav-windows.png)

In File Explorer if you open or have the navigation pane on the left you can also right click "This PC", choose "Show more options" and then "Map Network Drive". Choose a drive letter, type in http://centaur.local and check the box that says Reconnect at sign-in

![Mapping a driver](/assets/images/webdav-windows-map-drive.png)

Having a drive letter mapped can be useful for importing to chess analysis software that isn't aware of network drives.

## Accessing from a Mac

Open Finder. On the top menu click Go, Connect to Server. Then type in exactly http://centaur.local/ (or your centaur's name) and click Connect. Click connect again. Then choose "Guest"

![WebDav on Mac](/assets/images/webdav-mac.png)

## WebDav Applications

An alternative way of accessing is to use an application. I recommend Cyberduck https://cyberduck.io/download/ . But generally the methods above are better.

# Features

Obviously you can keep an access files on the DGT Centaur. But it has a few added features:

## Logging

In case you are having issues with the board, we now keep a log file called "debug.log" in the network share. This makes it easier to give support.

## PGN files

Up to the last 100 games will appear as PGN files in a folder called PGNs. They're read only. You can copy these to your computer or (in windows if you've mapped a drive) open them directly in your analysis software.

## Updates

Want to update to a specific dgt centaur mods version? Download the .deb file from the release page and put it on the network share. On the board go to Settings, Update Opts. The version number will be listed. Select it to start the update

## Custom scripts

We are pro your DGT Centaur board being your board to do what you like with. So we've made it even easier to do that and create custom scripts. Place a python .py file in the network share, shut the board down and turn it back on again and a new menu item called "Custom" will appear. If you select this, you can run any files you've put on the network share.
