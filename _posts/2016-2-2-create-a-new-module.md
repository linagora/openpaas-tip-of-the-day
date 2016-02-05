---
layout: post
title: Create a OpenPaas module
tip-number: 01
tip-username: Eriflow
tip-username-profile: https://github.com/Eriflow
tip-tldr: Creating an awesome OpenPaas module made easy.
---

OpenPaas is using a technological stack very closed to the [MEAN](http://mean.io/) stack. To be modular, we created a complete module framework allowing anyone to wrap its own  fullstack web application (express+angularjs) then include this project inside OpenPaas, getting finally access to the whole api.

### Getting started

First clone the **https://ci.open-paas.org/stash/projects/OM/repos/esn.helloworld** repository. It comes with a working OpenPaas module.

Let's quickly take a look at the directory layout:

```
backend/             --> All of the source files of the backend of your application
  lib/
  webserver/         --> All express specific files
    api/             --> Every exposed endpoints
    config/          --> All of the files of the configuration of express
    index.js         --> The main file describing the express server
frontend/            --> All of the source files of the backend of your application
  css/
    styles.less      --> The main stylesheet file
  js/
    app.js           --> The main angularjs application
  views/             --> All views
    index.jade
test/                --> All of the source files of the tests
  unit-backend/      --> backend tests
  unit-frontend/     --> frontend tests
index.js             --> The main module file, describing its capabilities/dependencies
package.json
bower.json
```

As you can see, this is a classic fullstack javascript project.
Take a particular look inside the root index.js file in which a few comments are explaining how this module could be enhanced.

### Integrate it

Now this is quiet simple. Either copy your new module inside **esn_repository_root/modules** or create a **symbolic link**. Then register your module in the **esn_repository_root/config/default.json** file under the **modules** key.

Be aware that using a **symbolic link** will require to run **npm install** in the original directory of you module.

```
  ...
  "modules": [
    "linagora.esn.account",
    "linagora.esn.appstore",
    "linagora.esn.calendar",
    "linagora.esn.contact",
    "linagora.esn.contact.import",
    "linagora.esn.contact.import.twitter",
    "linagora.esn.contact.twitter",
    "linagora.esn.core.webserver",
    "linagora.esn.core.wsserver",
    "linagora.esn.cron",
    "linagora.esn.davproxy",
    "linagora.esn.davserver",
    "linagora.esn.digest.daily",
    "linagora.esn.jobqueue",
    "linagora.esn.graceperiod",
    "linagora.esn.messaging.email",
    "linagora.esn.oauth.consumer",
    "linagora.esn.project",
    "linagora.esn.unifiedinbox",
    "esn.helloworld"
  ],
  "email": {
    "templatesDir": "./templates/email"
  }
  ...
```

Note that here we have registered the **esn.helloworld** module. The name of the module is defined in the root index.js file.

```javascript
var myAwesomeModule = new AwesomeModule('esn.helloworld', {
  dependencies: [
    new Dependency(Dependency.TYPE_NAME, 'linagora.esn.core.logger', 'logger'),
    new Dependency(Dependency.TYPE_NAME, 'linagora.esn.core.webserver.wrapper', 'webserver-wrapper')
  ],
  ...
}
```

### Test it

Run the esn, the module is automatically supported and started by OpenPaas. You should see a **My Module** new icon in the application grid (the last top right icon) that will lead you to a new view.

The module also add a new route. Let's issue a *curl* request.

```sh
$ curl http://localhost:8080/helloworld/api/sayhello
{"message":"Hello World!"}
```

It works! Finally, hack this module and enjoy.

You can find more information about the module system here:

- [The AwesomeModule framework](https://ci.open-paas.org/stash/projects/AM/repos/awesome-module-manager/)
- [dynamic-directive](https://github.com/linagora/dynamic-directive)
