Ktrl
====
**JavaScript Library for Web MIDI API**

Ktrl (/kənˈtroʊl/) is a JavaScript library that provides an abstract layer for all available MIDI input sources on the system and a convenient MIDI message routing system. It is built on top of [Web MIDI API](http://webaudio.github.io/web-midi-api/), which is currently available on the Chrome Canary build. (Version 30.0.1553.2 and beyond)

![Ktrl: How it works](etc/diagram.png "Ktrl: How it works")

In short, if you have a MIDI device(s) connected to your machine, you can start write up a MIDI-powered web app by including this library on your web page. With all the technical chores hidden, it just gives you a stream of MIDI data as JSON.

## Prerequisites
1. [MIDI controller(s)](https://www.google.com/search?q=MIDI+controller&source=lnms&tbm=isch&biw=1734&bih=1128&sei=q0fbUdlMwuWIArn1gfAJ)

2. Mac (at the moment. sorry.)

3. [Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html) build with Web MIDI API flag enabled

![Enabling MIDI API flag on Chrome Canary](etc/chrome-flag.png "Enabling MIDI API flag on Chrome Canary")

## How to use
```html
<script src="https://hoch.github.io/Ktrl/Ktrl.min.js"></script>
```

## Example usage
```javascript
// create and activate MIDI target (i.e. a synth)
var t = Ktrl.createTarget("mySynth");
t.activate();

// define MIDI data handler
t.onData(function (midimessage) {
  var data = Ktrl.parse(midimessage);
  console.log(t.label, data);
});

// prepare MIDI system and route it up
Ktrl.ready(function () {
  // route all MIDI inputs to the target
  Ktrl.routeAllToTarget(t);  
});
```

## Methods

#### Ktrl.report()

reports available MIDI sources and targets.

```javascript
> Ktrl.report()
  [ktrl] listing available MIDI sources...
  source 0   MIDI IN   KORG INC.
  source 1   PORT A    KORG INC.
  source 2   PORT B    KORG INC.
  source 3   Nocturn Keyboard    Novation
  [ktrl] listing available MIDI targets...
  target 0   mySynth   true
  target 1   myDrums   false 
```

#### Ktrl.createTarget(label)

creates a MIDI target with a label.

```javascript
var t = Ktrl.createTarget("mySynth");

t.onData(function (midimessage) {
  // here goes user-defined MIDI action
});
```

#### Ktrl.routeAllToTarget(target)

routes all available sources to a specified target. (all to one mapping) Note that this method can only be completed after Ktrl is ready. Use inside `Ktrl.ready()` method.

```javascript
Ktrl.ready(function () {
  // myTarget is defined beforehand...
  Ktrl.routeAllToTarget(myTarget);
});
```

#### Ktrl.routeSourceToTarget(sourceId, target)

routes a specified source to a target. (one to one mapping) Note that this method can only be completed after Ktrl is ready. Use inside `Ktrl.ready()` method. The source ID can be retrieved by pinging `Ktrl.report()`.

```javascript
Ktrl.ready(function () {
  // myTarget is defined beforehand...
  Ktrl.routeSourceToTarget(1, myTarget);
});
```

#### Ktrl.disconnectTarget(target)

disconnects a target from all sources keeping it available in Ktrl system.

```javascript
// myTarget is defined somewhere...
Ktrl.disconnectTarget(myTarget);
```

#### Ktrl.removeTarget(target)

removes a target from Ktrl system. also disconnects it from all the source beforehand.

```javascript
// myTarget is defined somewhere...
Ktrl.removeTarget(myTarget);
```

#### Ktrl.ready(function)

sets user-defined actions to be executed when Ktrl is ready.

```javascript
Ktrl.ready(function () {
  // do stuff here when Ktrl is done with probing MIDI assests
  Ktrl.report();
  Ktrl.routeAllToTarget(myTarget);
});
```

#### Ktrl.parse(MIDIMessage)

parses MIDI message into JavaScript-friendly form. A returned object varies according to the MIDI data type.

```javascript
["noteoff", "noteon"]: { type, channel, pitch, velocity }
["polypressure"]: { type, channel, pitch, pressure } 
["controlchange"]: { type, channel, control, value } 
["programchange"]: { type, channel, program } 
["channelpressure"]: { type, channel, pressure } 
["pitchwheel"]: { type, channel, wheel } 
```

Usually it should be used in conjunction with `target.onData()` method to specify the action for various MIDI data.

```javascript
// the target
var bullseye = Ktrl.createTarget("mySynth");

// define onData handler
bullseye.onData(function (midimessage) {
  var data = Ktrl.parse(midimessage);
  if (data.type === "noteon") {
    mySynth.noteOn(data.pitch, data.velocity);
  }
});
```

# Demo

- [Hello Ktrl!](https://hoch.github.com/Ktrl/examples/helloKtrl.html)

# Related resources

- [Web MIDI API W3C Editor's Draft](http://webaudio.github.io/web-midi-api/)
- [Web MIDI API Shim by Chris Wilson](https://github.com/cwilso/WebMIDIAPIShim)
- [Web Audio Demos by Chris Wilson](http://webaudiodemos.appspot.com/)

# License

Please find the license in the source code.