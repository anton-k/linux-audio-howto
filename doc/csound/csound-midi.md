Connect Csound to MIDI (Midi Sequencer driver)
================================

This driver is to be preferred over the Raw Midi driver. It has these advantages:

* Multiple concurrent access.

* Scheduled by priority queues.

* Real-time event dispatching i.e., the role of the Midi Sequencer is to deliver events at the right time (sequence) to the right destination (device).

The following command will call the Midi Sequencer. Here it listens to midi port 20. The midi output port is also 20:

~~~
-+rtmidi=alsaseq -M20 -Q20
~~~

Csound will automatically create its own ALSA sequencer port.
For a list of available devices, use the following command:

~~~
aconnect -i -o
~~~

This will create output that will look something like the following:

~~~
    client 0: 'System' [type=kernel]
    0 'Timer           '
    1 'Announce        '
client 14: 'Midi Through' [type=kernel]
    0 'Midi Through Port-0'
client 20: 'M Audio Delta 1010' [type=kernel]
    0 'M Audio Delta 1010 MIDI'
client 130: 'Csound' [type=user]
    0 'Csound
~~~

The output of Csound will contain lines like:

~~~
...........
ALSASEQ: opened MIDI output sequencer
ALSASEQ: created output port 'Csound' 130:0
ALSASEQ: connected to 20:0
..............
...........
ALSASEQ: opened MIDI input sequencer
ALSASEQ: created input port 'Csound' 130:0
ALSASEQ: connected from 20:0
..............
~~~