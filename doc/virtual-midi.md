Virtual midi
===============================

MIDI can be done two (or even more) ways on Linux:

    Alsa manages your midi devices
    Jack does

BWS currently only supports alsa MIDI directly. To make use of jack-only facilities and applications, you can bridge jack and alsa.


Virtual MIDI
--------------------------------

kernel module: **snd_virmidi**

Load the kernel module prior starting Bitwig Studio:

~~~
$ sudo modprobe snd_virmidi
~~~

This will give you (per default) four virtual midi devices, if you only want one, extend the command thus:

~~~
$ sudo modprobe snd_virmidi midi_devs=1
~~~

You'll have to tell Linux to reload that module after a reboot:

~~~
$ sudo su
~~~

You'll possibly have to enter your password now. Afterwards:

~~~
 $ echo snd-virmidi >> /etc/modules
~~~

Hit CTRL-D once to exit administrative (su) mode.

### Important: No Jack MIDI!

There is an option for jack (specifically in qjackctl -> lower right end of the settings dialog) to enable jack's midi support.

Don't do this! Jack will grab hold of all your midi devices and lock them, which means your
USB controllers won't be available directly to Bitwig Studio and will probably
break a lot of controller support. What? Yes, but via loophole

Instead, you can use a bridge to route midi data between both worlds: alsa2jack gateway: a2jmidid

Before starting Bitwig Studio, preferrably after starting the virtual midi system (see above),
launch the alsa <-> jack midi gateway daemon a2jmidid:

~~~
$ a2jmidid
~~~

This will enable you to route MIDI data freely between Bitwig Studio, Jack and Alsa devices
in Patchage or other routing tools.

Just make use of the newly appearing a2jmidi container in patchage, it translates red to
green connections. Bitwig MIDI

Notice, how with the many virtual devices, you can build complex operations and very
dynamic routing. Input

To get MIDI input into Bitwig, you can add a "Controller Script", e.g. a "Generic Keyboard"
and connect its input to your Virtual MIDI device.

### Routing

After you have started all applications that you want to route midi between, start Patchage,
which allows you to set up the routing between all the different bits and pieces.

Make sure you pick the right virtual device all the time and connect it via Patchage as you like.

### Output

To output MIDI data to devices or virtual synths, just use a common Hardware Instrument
and select the correct device for output again.
