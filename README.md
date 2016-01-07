# vue-meteor
Adds mixins and methods to Vue.js for easier use with Meteor.

## Installation

```
var Vue = require('vue')

Vue.use(require('vue-meteor'))
```

## Examples

Updates when reactive Meteor data sources change
```
module.exports = {  
  dataMeteor: {
    tasks: function() {
      return Tasks.find().fetch()
    }
  },
  created: function() {
      this.$subscribe('tasks')
  }
}
```

Updates when reactive Vue or Meteor data sources change
```
module.exports = {  
  data: function() {
    return {
      taskId: '123'
    }
  },
  dataReactive: {
    tasks: function() {
      return Tasks.findOne(this.taskId)
    }
  },
  created: function() {
      this.$subscribe('tasks')
  }
}
```

Similar to $watch (only tracks Meteor reactive data sources)
```
var untrack = this.$trackMeteor(function() { // called whenever Session.get('xyz') changes
  return Session.get('xyz')
}, function(value) { // called with return value (not reactive)
  // do something
})
```
```
untrack() // stop tracking
```

Similar to $watch (tracks Meteor and Vue reactive data sources)
```
var untrack = this.$track(function() { // called whenever Session.get('xyz') or this.abc changes
  return Session.get('xyz') + this.abc // this.abc is a Vue data object
}, function(value) { // called with return value (not reactive)
  // do something
})
```
```
untrack() // stop tracking
```

Subscriptions that stop when component is destroyed
```
var subscription = this.$subscribe('tasks')
```
