RiotControl
============

A Simplistic Central Event Controller / Dispatcher For [RiotJS](https://github.com/muut/riotjs), Inspired By Facebook's [Flux](https://github.com/facebook/flux) Architecture Pattern. 

RiotControl is, in the spirit of Riot itself, extremely lightweight. It forgoes elements of Flux, to favor small and simple applications. RiotControl passes events from views to stores, and back, relying heavily on Riot's observerable API. Stores can talk to many views, and views can talk to many stores.

Example data flow:

1. TodoList view triggers 'todo_remove' event to RiotControl.
2. RiotControl passes event along to stores.
3. TodoStore implements a 'todo_remove' event handler, talks to back-end server.
4. TodoStore triggers 'todos_changed' event, with new data.
5. TodoList view implements a 'todos_changed' handler, receiving new data.

Demo
============

TodoList demo built on Riot 2.0 demo:

http://jimsparkman.github.io/RiotControl/demo/

Reference todostore.js and todo.tag to understand how this works.

Usage
============

Requires Riot 2.0+

Include riotcontrol.js, or it's few lines of code, in your project.

API
============

Register the store in central dispatch, where store is a riot.observable(). Generally, all stores should be created and registered before the Riot app is mounted.

```javascript
RiotControl.addStore(store) 

// Example, at start of application:
var todoStore = new TodoStore() // Create a store instance.
RiotControl.addStore(todoStore) // Register the store in central dispatch.
```

Trigger event on all stores registered in central dispatch. Essentially, a 'broadcast' version of Riot's el.trigger() API.

```javascript
RiotControl.trigger(event)
RiotControl.trigger(event, arg1 ... argN)

// Example, inside Riot view (tag):
RiotControl.trigger('todo_add', { title: self.text })
```

Listen for event, and execute callback when it is triggered. This applies to all stores registered, so that you may receive the same event from multiple sources.

```javascript
RiotControl.on(event, callback)

// Example, inside Riot view (tag):
RiotControl.on('todos_changed', function(items) {
    self.items = items
    self.update()
})
```
