ktrl
====
**JavaScript Library for Web MIDI API**

Ktrl.js is a JavaScript library that provides an abstract layer for all available MIDI input sources on the system and a convenient MIDI message routing system. It is built on top of Web MIDI API, which is currently available on the Chrome Canary build. (Version 30.0.1553.2 and beyond)

## Prerequisites
1. MIDI controller(s)
2. Chrome Canary build with Web MIDI flag enabled

## How to use
```html
<script src="https://github.com/hoch/ktrl/raw/master/ktrl.js"></script>
```

## Example usage
```javascript
// create MIDI target (i.e. a synth)
var t = Ktrl.createTarget("mySynth");

// define MIDI data handler
t.onData(function (midimessage) {
  var data = Ktrl.parse(midimessage);
  console.log(t.label, data);
});

// prepare MIDI API and route up
Ktrl.ready(function () {
  // route all MIDI inputs to the target
  Ktrl.routeAllToTarget(t);
  
  // active target
  t.activate();
});
```

## Methods

### Ktrl.report()

### Ktrl.routeAllToTarget(target)

### Ktrl.routeSourceToTarget(sourceId, target)

### Ktrl.disconnectTarget(target)

### Ktrl.removeTarget(target)

### Ktrl.ready(function)

### Ktrl.parse(MIDIMessage)