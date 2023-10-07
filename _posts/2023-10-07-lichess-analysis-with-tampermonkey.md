---
layout: post
title:  "Lichess Analysis with Tampermonkey"
author: Ed
categories: [ Manual, Lichess ]
image: assets/images/4.jpg
---
Under [Tampermonkey User Script][tampermonkey-usercript] you'll find a Tampermonkey userscript. This allows you to quickly import some of the latest PGNs on your board over wifi into the Lichess Analysis tool to view and do your analyis, in for example Chrome or Firefox.

First make sure you have installed Tampermonkey: [Tampermonkey][tampermonkey]

In Tampermonkey in your browser click "Create New Script" and copy and paste the contents in.

Adjust the line in the script that reads:

var boardname = 'centaur.local';

To the address of your DGT Centaur. Then save the script.

Go to Lichess analysis, or refresh if you are already there.

Clicking in the PGN textbox should produce a new button "DGT Centaur". Clicking on this will bring up a listing of your latest games. If you choose one, it will load the PGN into the textbox over wifi and start the analysis.

[tampermonkey-usercript]: [(https://github.com/EdNekebno/DGTCentaurMods/tree/master/tools/tampermonkey]
[tampermonkey]:   [https://www.tampermonkey.net/]
