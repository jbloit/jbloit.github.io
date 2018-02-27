---
layout: post
title: "super collider cheat sheet"
date: "2018-02-25 18:56:44 +0100"
---

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

# resources
- [The SuperCollider book](http://supercolliderbook.net/)
