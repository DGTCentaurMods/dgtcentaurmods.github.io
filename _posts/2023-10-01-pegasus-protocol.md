---
layout: post
title:  "Pegasus Protocol"
author: Ed
categories: [ Pegasus, Protocols ]
image: assets/images/manual-green.png
---

This document describes the DGT Pegasus Protocol. The protocol described here is reverse engineered, and thus not the official version. Some information may be unclear or incomplete in comparison to the DGT API, however this lets you talk to the board with no commitments.

The DGT Pegasus is a BLE board, it uses a standard Nordic UART to provide serial functions. Nordic UART has the following UUIDs:

Nordic UART Service is 6E400001-B5A3-F393-E0A9-E50E24DCCA9E
RX Characteristic (UUID: 6E400002-B5A3-F393-E0A9-E50E24DCCA9E)
TX Characteristic (UUID: 6E400003-B5A3-F393-E0A9-E50E24DCCA9E)

Where RX is the receive for the board (i.e. your app or program sends to this) and TX transmits data from the board (to your app or program). Data is received with Notify.

## Data Returned from the Board

Packets of data from the board always take the same form. The first byte represents a message type, the second byte is always 0, the third byte represents the entire length of the packet, followed by a variable number of bytes in a packet which consist of the data. For example, a hardware version may return:

[150, 0, 5, 1, 0]

Here 150 indicates the message is the hardware version. 0 always occurs, 5 indicates the entire packet length is 5 bytes. The last two bytes represent the version number = 1.0

Some codes are unknown but return responses, or they may just not be fully implemented. Therefore I list each code out individually even if they are unknown:

## Board Dump 134

Results in a bitboard representation of the board and where the pieces are. First 3 bytes are therefore 134, 0, 67 . There follows 64 bytes which are either 0 or 1. 1 if there is a piece on the square, 0 if not. These work from the top left, across, and down. So the first byte represents a8 and the last represents h1.

## FIELD_UPDATE 142

Provides notification that a piece has been lifted or placed down on a field. First 3 bytes are 142, 0, 5 . Last two bytes are the field number (0 being top left a8, 63 being bottom right h1) followed by 1 for piece placed on board or 0 for piece lifted from board

## UNKNOWN 143

As yet unknown. However, shortcommand I to the board returns this. This has no data so it returns 143, 0, 3

## UNKNOWN 144

As yet unknown. However, shortcommand F to the board returns this

## SERIALNR 145

Returns a serial number for the board. This does not seem to be related to the actual serial number printed on the board! First 3 bytes are 145, 0, 8. There follows 5 ASCII characters

## TRADEMARK 146

Returns board manufacturer, copyright, software version, hardware version, etc as ASCII string with 13, 10 as newlines.

Digital Game Technology
Copyright (c) 2021 DGT
software version: 1.00, build: 210722
hardware version: 1.00, serial no: PXXXXXXXXX

Chess.com app and DGT app check text in this to validate that it is a pegasus board.

## VERSION 147

Returns version number (presumably software). First three bytes are 147, 0, 5 . Following two bytes are whole value, fractional value. For example 1, 0 is 1.0

## HARDWARE_VERSION 150

Returns the hardware version. First three bytes are 150, 0, 5 . Following two bytes are whole value, fractional value. For example 1,0 is 1.0

## BATTERY_STATUS 160

Returns information about battery charge and connection to charger. First three bytes are 160, 0, 12 . Next byte is battery charge where 88 is the maximum value ( to convert to % take the (value * 114)/100 ). There follows 7 0s. Last byte is 1 when charging.
LONG_SERIALNR 162

Returns the board serial number. First three bytes are 162, 0, 13. Following are 10 bytes as ASCII for the serial number.

## UNKNOWN 163

As yet unknown. However, shortcommand V to the board returns this

## LOCK_STATE 164

Returns 164, 0, 4 followed by 0. Presumably the 0 means unlocked

## DEVKEY_STATE 165

Returns status of the developer key 165, 0, 4, 0 . Presumably 0 means okay

## COMMANDS TO THE BOARD

Commands to the board take two formats. For control of the LEDs a protocol similar to that in use in the DGT Centaur is used. For other commands a shortcommand format is used which consists of a single ASCII character. Shortcommands will generally result in the board sending back a data containing a message as defined above.

## SHORTCOMMANDS

@ - Reset. It is a reset but doesn't not seem to reset the board. Presumably sets the board to an initial state as it is sent before each game.

B - Board dump. The board will send back a BOARD_DUMP message representing the bitboard state

D - Asks the board to start sending FIELD_UPDATE messages when a piece is lifted or placed on the board

E - Serial Number. The board will send back a SERIALNR message

F - Returns UNKNOWN 144 message

G- Trademark. The board will send a TRADEMARK message

H - Hardware Version. The board will send back a HARDWARE_VERSION message

I - Returns UNKNOWN 143

L - Battery Status. The board will send back a BATTERY_STATUS message

M - Version. The board will send back a VERSION message

U - Long Serial Number. The board will send back a LONG_SERIALNR message

V - Returns Unknown 163

Y - Lock State. The board will send back a LOCK_STATE 164 message

Z - Checks if the device is authorised. Returns DEVKEY_STATE message

## LED COMMANDS

For flashing the LEDS we simply need to know two commands.

The byte sequence 96, 2, 0, 0 will turn off all leds

To turn on LEDs we use a similar but more complex sequence:

96, packetlength - 2, 5, LedSpeed, blink, intensity, ... field ids, 0

LedSpeed should be a number between 1 and 7, it represents how fast the LEDs flash. In practice it can exceed this range and go as high as 25 but probably at your own risk! blink - if set to 0 then the LED will continue to pulse, if set to 1 it will flash once (for example when the board flashes to confirm you have placed a piece down on a field). intensity - the brightness. Should be between 1 and 4. I haven't tested a max as 1 is always plenty bright! ...field ids - one or more field ids to flash. If just blinking a confirmation this would be one field id, if showing moves it would be two. Any number of field ids seems possible though. Remember to set the second byte to the number of bytes in the packet less 2. Note: the last byte is always zero.

Some examples:

Blink LED on c8 - 96, 6, 5, 7, 1, 1, 2, 0
96 = LEDs
6 = There are 6 bytes after this byte
5 = On
7 = Fast
1 = Flash once
1 = Intensity
2 = c8
0 = always 0

Indicate move a8 to a7 - 96, 7, 5, 7, 0, 1, 0, 8, 0
96 = LEDs
7 = There are 7 bytes after this byte
5 = On
7 = Fast
0 = Continuous pulse
1 = Intensity
0 = a8
8 = a7
0 = always 0

## OTHER

Some sequences seem not strictly necessary but always sent, these could perhaps be implemented in the future. It's therefore probably wise to also send them.

On a new game the shortcommand @ is sent, followed by the shortcommand D, followed by 96, 2, 0, 0 to turn off the lights. This presumably ensures the board is in a known starting state.

On connection the board likes to register a Developer's key. The Bitboard returns all 0x7Fs and lights do not work until this is sent. the DGT Chess App key is sent openly so you can just send that :) To send the key to the board use the byte sequence 99 7 190 245 174 221 169 95 0 .

101 [length of bytes after this byte] ASCII String of Bytes 0 - Set Board Name
