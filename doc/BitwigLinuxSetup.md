Bitwig: Linux audio setup
===============================

Hello! This mini howto expects that you're on familiar grounds with terminals and filesystems
and that you know how to install software on your distribution. If you're not, just comment below,
so other users can benefit from your questions.

Sound on Linux works like this:

    Alsa is Linux' preferred hardware - or card - backend. Better not to be used by BWS directly, though!
    Jack is used to bring realtime performance and flexible routing into the game.

So in essence, you configure your soundcard with alsa, then run jack and connect BWS to it.

You may want to use a terminal to follow these steps, but there are graphical tools to do most of what this howto covers.

To install all the necessary packages - and a few other goodies - on Ubuntu or Debian derivatives in general, use this command:

~~~
$ sudo apt-get install alsa-tools alsa-tools-gui alsamixergui patchage jackd2 \
  jackd2-firewire qjackctl a2jmidid gmidimonitor
~~~

## Card backend: alsa

Most current soundcards and a lot of older ones are supported out of the box. Some Firewire cards can be tricky.

Most other audio software should either work with alsa or jack as backend.

Here's some pointers you can check for compatibility issues:

* [Card compatibility matrix](http://www.alsa-project.org/main/index.php/Matrix:Main)

* [Software matrix](http://www.alsa-project.org/main/index.php/Applications)

### Tools

There are some alsa backend tools that you can use to check working state and debug problems:

* **alsaplayer**

Straightforward: plays audio files, there is a graphical version, too, but the command line version
can easily tell you whether your card and setup works, just use this command in a console:

~~~
$ aplay -l
~~~

..and it will lookup your soundcards and list them if they're supported.


*  **alsamixer**

After verifying that your card(s) work, you'll want to setup their mixers according to your setup.

**Beware**: At least check your mixer settings before ignoring this step! You might lateron
find out that default settings for your mixer is all channels muted or set to zero.

You can use either `alsamixer` (console tool) or `alsamixergui`, which comes with a graphic display:

~~~
$ alsamixer
~~~

or

~~~
$ alsamixergui
~~~

Since your going to use a digital audio workstation, we'll assume, you're familiar with mixers.

### Other tools

* Storing mixer settings: **alsactl**

**Beware**: Because the mixer might be reset after reboot, use the `alsactl` tool to take snapshots of your mixers.
Note: this requires administrator access, e.g. via your password!

Save:

~~~
$ sudo alsactl --file ~/.config/mixer.state store
~~~

Restore:

~~~
$ sudo alsactl --file ~/.config/mixer.state restore
~~~

* View sequencer data live: **aseqdump**

To make sure, your MIDI data is sent and received correctly, if often helps to poke around alsa's midi system,
you can use `aseqview` to debug problems here, but we'll cover another more graphical tool later.

~~~
$ aseqdump
~~~

* Basic alsa midi routing: `aconnectgui`

Using `aconnectgui`, you can adjust basic midi connections in alsa, but again, we'll cover another more convenient tool, later on.

~~~
$ aconnectgui
~~~

## Audio backend: jack

Since alsa isn't meant for professional audio requirements, its routing and multiplexing capabilities are rather limited,
or better "static" - you can configure it to offer lots of channels and mix in software - but you probably don't want to.

So to make use of Linux' realtime capabilities and to get professional mixing and routing, you want to use Jack as audio backend.

This can be done in a multitude of ways, we'll just cover the most basic ones, that are more or less guaranteed to work on every (Linux) platform.

### jackd

If you only have one soundcard, the easiest way to start jack is thus:

~~~
$ sudo jackd -R -d alsa -r 96000
~~~

If you have more than one card or need special settings, it can become messy.
You could lookup all of the necessary parameters in jack's manpage. But there is an easier way.

**Beware**: If this doesn't work, you may have either a systemwide jackd running
(use qjackctl in that case and instruct it to use dbus), or something else - like another
audio application - is blocking your soundcard. Jack is meant to alleviate these problems
by enabling sharing of alsa resources but can still sometimes be blocked itself.

### qjackctl

For complex setups, it makes sense to use `qjacktctl`, the graphical jack setup tool.
It makes life a little easier e.g. by offering a drop down combobox of your sound hardware, usual buffer sizes, etc.

## Bitwig Audio Engine

Once you have jack running and verified it is working as expected, set up BWS to use Jack as audio engine backend.


## Further reading:

* [How to test your soundcard with alsa](http://www.alsa-project.org/main/index.php/SoundcardTesting)

* [Jack Quickstart Guide on Ubuntu Communities](https://help.ubuntu.com/community/UbuntuStudio/JackQuickStart)

* [What is Jack? on Ubuntu Communities](https://help.ubuntu.com/community/What%20is%20JACK)

* [Linux Professional Audio Wiki](https://help.ubuntu.com/community/What%20is%20JACK)



