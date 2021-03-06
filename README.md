# emitter-on-steroids

Not your average emitter. An event emitter with the following features:

- global listeners
- synchronous and asynchronous listeners
- error handling
- formal events, stoppable events
- works in the browser too

## Example

### Creating an instance

	var emitter1 = emitter();
	
or
	
	var emitter2 = new emitter();

or
	
	var MyClass = function() {}
	emitter(MyClass.prototype);
	var emitter3 = new MyClass();

### Global listeners

Global listeners are registered with the special wildcard name and are called for every event emitted. Global listeners 
are called before regular listeners.

	emitter()
	  .on('*', function() {
	    console.log('listener #1');
	  })
	  .emit('test')
	;

### Synchronous and Asynchronous listeners

	emitter()
	  .on('test', function() {
        console.log('listener #1 - synchronous');
      })
	  .on('test', function(done) {
	  	setTimeout(function() {
	  		console.log('listener #2 - asynchronous');
	  		done();
	  	}, 1);
	  })
	  .emit('test', function() {
	    //called after all listeners have finished
	  })
	;

### Formal events

	var Event = require('emitter-on-steroids/event');

	emitter()
	  .on('test', function(event) { //the event object is passed as the first argument
	    console.log('listener #1');
	  })
	  .on('test', function(event, done) { //the event object is passed as the first argument
	    console.log('listener #2');
	    done();
	  })
	  .emit(Event.stoppable(new Event('test')), function(err, event) { //the event object is passed as the second argument

	  })
	;

### Stoppable events

	var Event = require('emitter-on-steroids/event');

	emitter()
	  .on('test', function(event) {
	    console.log('listener #1');
	    event.stopPropagation();
	  })
	  .on('test', function() {
	    console.log('listener #2');
	  })
	  .emit(Event.stoppable(new Event('test')))
	;


### Error Handling
	
#### Synchronous

	emitter()
	  .on('test', function() {
	    console.log('listener #1');
	    throw new Error('A test error');
	  })
	  .emit('test', function(err) {
	    console.log('done', err);
	  })
	;

#### Asynchronous

	emitter()
	  .on('test', function(event, done) {
	    console.log('listener #1');
	    done(new Error('A test error'));
	  })
	  .emit('test', function(err) {
	    console.log('done', err);
	  })
	;


## API

### EventEmitter

#### EventEmitter() or new EventEmitter()

Create a new emitter instance.

#### emitter(object : Object)

Mixin emitter methods into object.

#### .on(name : String, listener : Function(Event, [done : Function()]))

Register a listener to receive notifications each time an event is emitted.

#### .once(name : String, listener : Function(Event, [done : Function()]))

Register a listener to receive notifications the next time an event is emitted.

#### .off(name : String, listener : Function(Event, [done : Function()]))

Un-register a listener from receiving notifications each time an event is emitted.

#### .emit(event : String|Event, [arg1, arg2, ...], [done : Function(Event)]))

Notifies listeners that an event occurred.

### Event

#### .getName()

Get the event name.

#### .getEmitter()

Get the event emitter.

#### #preventable(object)

- .isDefaultPreventable() - Get whether the default event action has been prevented.
- .preventDefault() - Prevent the default event action from executing.

#### #stoppable(object)

- .isPropagationStopped() - Whether the event has been stopped from propagating.
- .stopPropagation() - Stops the event from propagating.

## License

The MIT License (MIT)

Copyright (c) 2014 James Newell

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
