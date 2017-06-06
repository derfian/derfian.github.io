---
title: Upgrading the Techart TA-GA3 Contax G - NEX adapter
layout: post
---

The Techart TA-GA3 is a lens adapter made to fit the Sony NEX or
α-series mirrorless cameras. It allows you to mount lenses made for
Contax G, and the adapter includes its own autofocusing system for
these lenses. Techart offers firmware updates to this adapter via an
[Android](https://play.google.com/store/apps/details?id=com.techart.updateall)
and [iOS](https://itunes.apple.com/us/app/techart-update/id972383385)
app called TECHART Update. The app communicates with the adapter via
Bluetooth.

To upgrade the adapter, put the camera in manual mode, set the
aperture on the camera to 𝑓/90, take a picture and finally turn off
the camera. This will put the adapter in the "programming" mode, and
it will turn on its Bluetooth connection. Some instructions online
says you should also remove the lens, although in my experience this
is not necessary.

In the app, you select the firmware version you want, and the app will
then download the firmware, discover the adapter and then start
updating the adapter with the new firmware. When everything works
properly, this is a very nice way of handling firmware updates.

Inside the menu system of the Sony α7 camera, there is a field where
the lens firmware version is shown. I've been experimenting with the
firmware version but I've never seen it actually change from the value
"2" for this adapter. So in a misguided attempt to see whether it
showed, I decided to downgrade the TA-GA3 adapter to the oldest
available firmware for it, firmware version 1.0.0.

While the downgrade went just fine, ever since then the TECHART Update
app, neither on Android nor iOS, was able to discover the adapter to
upgrade it again.

> Adapter TA-GA3 not found, please try again

After a few months of not really bothering too much with it, I sat
down yesterday to figure out what was actually going on. I tried to
put the adapter in the firmware update mode, and I saw something
called "EOS-iNEX II" show up as an available bluetooth device on my
phone.

I quickly concluded that the TA-GA3 1.0.0 firmware makes the adapter
identify as a EF mount to NEX adapter. It's no wonder the TECHART
Update app didn't find any TA-GA3 adapters to update! Now that I had a
good idea about the problem, how do I convince the Techart Update app
to put a firmware for the *TA-GA3* adapter onto an adapter that
identifies itself as a *EOS-iNEX II*?

My first idea was to reverse engineer the TECHART Update app and patch
it to allow it to patch any device with any firmware. I started
looking at Techart Update app and I found the string
http://www.techart-logic.com/g-nex/firmware/firmware.txt. This CSV
file contains a list of firmwares for different models of lens
adapters. The fields in the file were adapter, description, url,
unknown.

Armed with that information, I created a copy of the firmware.txt file
and modified it to apply a *TA-GA3* firmware to a *EOS-iNEX II*
adapter. I created a custom DNS zone for techart-logic.com on my own
network, and pointed it to a web server that would serve the files as
specified in the firmware file.

And... it worked! The TECHART Update app read my own firmware.txt
file, presented me with the option to flash a *TA-GA3* firmware on a
*EOS-iNEX II* adapter, and once I selected it, the app found the
adapter straight away and started updating it. A couple of minutes
later, it was done.

