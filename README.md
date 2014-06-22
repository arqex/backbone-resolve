backbone-resolve
================

Backbone-resolve adds permanent events to Backbone.js. [Permanent events](http://arqex.com/900/permanent-events-javascript) are a special kind, after they have been triggered any listener added to them get inmediately executed.

Backbone-resolve extend the Backbone.Events object with a method ```resolve``` that defines a permanent event. Any listener added to a resolved event is getting automatically called, like if the event was triggered in that same instant. An example
```javascript
// First, extend our object with events
// thanks to underscore
var myobject = _.extend({}, Backbone.Events);

// It is possible to add listener before the event is resolved
myobject.on('ready', function(){
    console.log('I was bound before the event was resolved.')
});

// trigger the permanent event 'ready'
myobject.resolve('ready'); 
// Console will show 'I was bound before the event was resolved.'

// listening to the event now, 
// will execute the callback inmediatelly
myobject.on('ready', function(){
    console.log('I was bound after the event was resolved.');
});
// Console will show 'I was bound after the event was resolved.'
```

Extending the Backbone's Event module, backbone-resolve can be used out-of-the-box with BB Models and Views.
```javascript
var mymodel = new Backbone.Model();

// Let's resolve the fetched method when we
// retrieve model's data
mymodel.fetch({success: function(){
    mymodel.resolve('fetched');
}});

// Any listener added in the future will
// be called if the model has been fetched
mymodel.on('fetched', function(){
    console.log('Awesome!!')
});

```

## License
Licensed under MIT. [See license](https://raw.githubusercontent.com/arqex/backbone-resolve/master/LICENSE)

## How to use it
The method ```resolve``` was developed to be used like Backbone's ```trigger``` method. It accepts parameters that will be arguments for the listeners bound.
```javascript
var myobject = _.extend({}, Backbone.Events);

myobject.resolve('argument:test', 'Hello', 'bye');

myobject.on('argument:test', function(arg1, arg2){
    console.log(arg1 + ' and ' + arg2 + ' world.');
});
// Console will show 'Hello and bye world.'
```

You can use usual Backbone ```on```, ```once```, ```listenTo``` and ```listenToOnce``` methods to bind listeners to permantent events.

If there is a point that the event data is not valid anymore, the permanent events can be discarded using the method ```discard```. After using it, following added listener will not be executed inmediately.
```javascript
myobject.discard('argument:test');

myobject.on('argument:test', function(arg1, arg2){
    console.log(arg1 + ' is the opposite of ' + arg2);
});
// Nothing will be printed, you need to trigger or resolve 'argument:test'
// again because the permanent event was discarded.
```

## Changelog
**v0.2.1**
First version published.