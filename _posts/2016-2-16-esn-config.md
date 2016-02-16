---
layout: post
title: How to get and set configurations in the ESN
tip-number: 06
tip-username: Eriflow
tip-username-profile: https://github.com/Eriflow
tip-tldr: Introduction of the powerful esn-config module
---

Except for the configuration of MongoDB, every other configurations of OpenPaas are stored in a "configuration" collection inside MongoDB. We are providing a module to manage this collection and here is how to use it.

### The esn-config module

First you have to ensure that your module depends on **linagora.esn.core.esn-config** by adding a new line to your module definition file:

```javascript
var myAwesomeModule = new AwesomeModule('mymodule', {
  dependencies: [
    ...,
    new Dependency(Dependency.TYPE_NAME, 'linagora.esn.core.esn-config', 'esn-config'),
    ...
  ],
  ...
}
```

This module is a wrapper around [mongoconfig](https://www.npmjs.com/package/mongoconfig), using the connection provided by [mongoose](http://mongoosejs.com/).

Let's take a look at the API.

### Getting a new configuration

First we want to read the configuration of a specific key, let's say **redis**. This is what you will write in your code:

```javascript
module.exports = function(dependencies) {
  var esnconfig = dependencies('esn-config');

  function getRedisConfiguration(callback) {
    esnconfig('redis').get(function(err, config) {
      if (err) { return callback(err); }
      if (!config) {
        return callback(new Error('No "redis" configuration has been found'));
      }
      return callback(null, config);
    });
  }

  return {
    getRedisConfiguration: getRedisConfiguration
  };
};
```

As you can see, it is pretty simple.

### Setting a new configuration

Now this time you want to write a new configuration key, about your other server the esn will have to communicate with. In you code, you will write:

```javascript
module.exports = function(dependencies) {
  var esnconfig = dependencies('esn-config');

  function setServerConfiguration(callback) {
    var config = { host: 'myserver.com' };
    esnconfig('mymodule-server').store(config, function(err) {
      if (err) { return callback(err); }
      return callback(null);
    });
  }

  return {
    setServerConfiguration: setServerConfiguration
  };
};
```

That's it, you can now use **esn-config** in your module and play with every configuration keys of OpenPaas.

You can find more informations here:

- [mongoconfig](https://www.npmjs.com/package/mongoconfig)
- [The list of existing configuration keys](https://github.com/linagora/openpaas-esn/blob/master/doc/configuration.md)
