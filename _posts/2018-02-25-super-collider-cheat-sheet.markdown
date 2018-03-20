---
layout: post
title: "super collider cheat sheet"
date: "2018-02-25 18:56:44 +0100"
---

Survival kit when starting out with super collider - as I am.

### Run a command
- Click to place the cursor somewhere on the same line as the code and then press ```Shift-Enter```
- stop: ```cmd-.```

### Print to console
```
"Hello World!".postln;
```

### Execute a Block
double clicking on first line (parenthesis) selects all.
```
(
"Call me,".postln;
"Ishmael.".postln;
)
```
when selected, run block with ```cmd-Enter```

### Server
- boot: ```cmd-b```
- object: ```s```

### Functions
use curly brackets to define a function object.
- ```f = { "Function evaluated".postln; };
f.value;```

All objects in SC respond to the message 'value'
 When you call the method 'value' on a Function it will evaluate and return the result of its last line of code

- ```f.play```: evaluate and play on a Server

- arguments:
```
f = { arg a, b;
    a - b;
};
f.value(5, 3);
```

- can be called with keyword args:
```
f = { arg a, b;
    a - b;
};
f.value(5, 3);
```
- default values:
```
f = { arg a, b = 2; a + b; };
```

- arguments can be enclosed in pipe symbols. f and g are equivalent:
```
f = { arg a, b; a + b; };
g = { |a, b| a + b; };
f.value(2, 2);
g.value(2, 2);
```

### Variables
- declared with ```var```
```
f = { arg a, b;
    var firstResult, finalResult;
    firstResult = a + b;
    finalResult = firstResult * 2;
    finalResult;
};
```
- interpreter variables: The letters a to z are what are called interpreter variables. These are pre-declared when you start up SC, and have an unlimited, or 'global', scope. This makes them useful for quick tests or examples.

- environment variables: create / access variables outside the current scope. Access with tilde key character;

[From the forums](http://new-supercollider-mailing-lists-forums-use-these.2681727.n2.nabble.com/global-variables-td7421932.html):

```
The tilde doesn't declare a variable, it's just a shortcut for a lookup in the currentEnvironment (a Dictionary-like structure).

~c = ~a + ~b; // will only work when ~a/~b are set

translates (almost) to:

currentEnvironment['c'] = currentEnvironment['a'] + currentEnvironment['b']; // now ~c is the sum of ~a and ~b

Please also note, that a and ~a are not the same (a is a global variable, ~a is an environment variable):

~a = 10;
a = 5;

~a + a;  // 15
```

## UGens
These are objects which produce audio or control signals
- audio rate:
```
{ SinOsc.ar(440, 0, 0.2) }.play;
```
- control rate:
```
{ SinOsc.kr(440, 0, 0.2) }.play;
```
- mul : ugen argument, multiplies output with mul value
- basic sin:
```
(
{             // Open the Function
    SinOsc.ar(    // Make an audio rate SinOsc
        440,     // frequency of 440 Hz, or the tuning A
        0,     // initial phase of 0, or the beginning of the cycle
        0.2)     // mul of 0.2
}.play;            // close the Function and call 'play' on it
)
```

## Math
SC has no operator precedence, i.e. math operations are done in order, and division and multiplication are not done first. To force an order use parentheses. e.g.
```
4 + (2* 8)
```

## External control

### OSC
- check incoming port
```
NetAddr.langPort;
```
will post the port SC is listening to. By default, this is ```57120```.

- this example prints the received OSC message and other info when receiving "/main/toggle":
```
(
s.boot;
OSCdef.new(\toggle,
	{|msg, time, addr, recvPort|
		[msg, time, addr, recvPort].postln;
}, '/main/toggle' );
)
```
### MIDI

- connect all devices:
```
MIDIClient.init;
MIDIIn.connectAll;
```
- list sources and only connect one device:
```
MIDICient.sources;
MIDIIn.connect(0);
```
- map events to a function

```
MIDIdef.noteOn(\noteOnTest, {"keydown".postln});
MIDIdef.cc(\ccIn, {"ccIn".postln});
```

- parse the event using args in function :

```
(
MIDIdef.cc(\ccIn, {
	arg key, func, ccNum, chan, srcID, argTemplate, dispatcher;
	key.postln;
});
)
```

- free a midi def callback:
```
MIDIdef.free(\ccIn);
```




# resources
- [The SuperCollider book](http://supercolliderbook.net/)
