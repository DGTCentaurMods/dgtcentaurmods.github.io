---
layout: post
title:  "Pegasus Protocol"
author: Ed
categories: [ Pegasus, Protocols ]
image: assets/images/6.jpg
---

This document describes the DGT Pegasus Protocol. The protocol described here is reverse engineered, and thus not the official version. Some information may be unclear or incomplete in comparison to the DGT API, however this lets you talk to the board with no commitments.

The DGT Pegasus is a BLE board, it uses a standard Nordic UART to provide serial functions. Nordic UART has the following UUIDs:

Nordic UART Service is 6E400001-B5A3-F393-E0A9-E50E24DCCA9E
RX Characteristic (UUID: 6E400002-B5A3-F393-E0A9-E50E24DCCA9E)
TX Characteristic (UUID: 6E400003-B5A3-F393-E0A9-E50E24DCCA9E)

Where RX is the receive for the board (i.e. your app or program sends to this) and TX transmits data from the board (to your app or program). Data is received with Notify.

##Data Returned from the Board

Packets of data from the board always take the same form. The first byte represents a message type, the second byte is always 0, the third byte represents the entire length of the packet, followed by a variable number of bytes in a packet which consist of the data. For example, a hardware version may return:

[150, 0, 5, 1, 0]

Here 150 indicates the message is the hardware version. 0 always occurs, 5 indicates the entire packet length is 5 bytes. The last two bytes represent the version number = 1.0

Some codes are unknown but return responses, or they may just not be fully implemented. Therefore I list each code out individually even if they are unknown:

##Board Dump 134

Results in a bitboard representation of the board and where the pieces are. First 3 bytes are therefore 134, 0, 67 . There follows 64 bytes which are either 0 or 1. 1 if there is a piece on the square, 0 if not. These work from the top left, across, and down. So the first byte represents a8 and the last represents h1.

