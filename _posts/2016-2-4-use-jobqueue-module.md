---
layout: post
title: Use jobqueue module
tip-number: 03
tip-username: BTNgo
tip-username-profile: https://github.com/btngo
tip-tldr: Creating and managing job jobqueue module.
---

### Prerequisite
[Create an OpenPaas module](https://github.com/linagora/openpaas-tip-of-the-day/blob/gh-pages/_posts/2016-2-2-create-a-new-module.md)

### What does this module do ?

This module provides APIs to create and manage queue job in OpenPaaS ESN.

### Add jobqueue module in the dependencies of your module

```javascript
var myAwesomeModule = new AwesomeModule('esn.helloworld', {
    dependencies: [
    ...
    new Dependency(Dependency.TYPE_NAME, 'linagora.esn.jobqueue', 'jobqueue')
    ],
    ...
}
```

### Adding a worker

Any module which import the jobqueue dependency can submit jobs in the jobqueue

The worker object to register is defined as:

```javascript

dependencies('jobqueue').lib.workers.add({
    name: 'contact-twitter-import',
    getWorkerFunction: function() {
    return yourWorkerFunction;
    }
})

```

### Calling a worker to do his job

Once registered, you can call worker job by his name and data:

```javascript

dependencies('jobqueue').lib.startJob(workerName, jobName, jobData);

```

### Job object

To get job object by his id:

```javascript

dependencies('jobqueue').lib.getJobById(jobId);

```

You can manage your queued job in this page: http://localhost:8080/jobqueue/ui/inactive

#### Reference

- [Automattic kue](https://github.com/Automattic/kue)