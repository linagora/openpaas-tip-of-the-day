---
layout: post
title: How to ease development with docker-compose
tip-number: 06
tip-username: chamerling
tip-username-profile: https://github.com/chamerling
tip-tldr: How to ease your OpenPaas ESN development with docker-compose
---

We are big fans of [docker](https://docker.com) and all the ecosystem/tools around it. Docker can really ease our daily development workflow, here is a tip on how to use docker-compose to run all the required ESN services without effort.

First, install the fresh new version of docker and docker-compose. We use docker v1.10.0 and docker-compose v1.6.0 in this tip.

Clone the OpenPaaS ESN repository from [https://github.com/linagora/openpaas-esn.git](https://github.com/linagora/openpaas-esn.git).

```bash
$ git clone https://github.com/linagora/openpaas-esn.git
```

Install npm and bower dependencies:

(We assume that you have node (v0.10.36) and so npm installed, if not check the awesome [nvm](https://github.com/creationix/nvm))

```bash
$ cd openpaas-esn
$ npm install
$ # take a coffee or two, check your mails, twitter streams...
```

Once everything is ready, you can launch MongoDB, Redis, ElasticSearch, SabreDAV, with a single command:

```bash
ESN_HOST=192.168.X.Y docker-compose -f ./docker/dockerfiles/dev/docker-compose.yml up
```

**Note :** 192.168.X.Y is your local network computer IP address. Do not put localhost nor 127.0.0.1 since some services will have to call your ESN instance and using a local IP will fail. Use a IP address the ifconfig command will return.

Once services are started, you have to configure the ESN to use these docker services.
We will refer to the YOUR\_DOCKER\_IP variable in the following instructions, replace this with your docker IP which should be '127.0.0.1' or 'localhost' on Linux and '192.168.99.100' on OS X/Windows (docker-machine based installation).

Let's first generate local configuration files:

```bash
$ node ./bin/cli.js db --host <YOUR_DOCKER_IP>
```

- YOUR\_DOCKER\_IP is the IP where MongoDB launched by docker-compose above can be reached (default to localhost, check the note above)

Once generated, let's inject the required configuration into MongoDB:

```bash
$ node ./bin/cli.js docker-dev --host <YOUR_DOCKER_IP>
```

- YOUR\_DOCKER\_IP is the IP where MongoDB launched by docker-compose above can be reached (default to localhost, check the note above)

Everything is now ready, you can start the OpenPaaS ESN in dev mode:

```bash
$ grunt dev
```

And browse [http://localhost:8080](http://localhost:8080). You should be able to login with the 'admin@open-paas.org' username and 'secret' password.

This is it! You are now able to hack on the OpenPaaS ESN code without having to install nor configure external services by hand.
