---
title: "Testing emails during development"
layout: post
---

Not being able to test (easily) every possible feature of your project or product during development is not only frustrating but could lead to unexpected results in production!

One of those tricky features is testing the outgoing e-mails. Why? Because you need to have a working SMTP server on your machine, and you cannot expect that everyone is willing or able to set this up. And even if they do, you still need valid e-mail addresses, etc! 

So how can we fix that?

Say hello to [MailDev](https://github.com/djfarrelly/MailDev "MailDev"):

> MailDev is a simple way to test your project's generated emails during development with an easy to use web interface that runs on your machine built on top of Node.js.

![MailDev UI](/images/posts/2017/maildev.png)

This neat tool, will allow you to very easily setup the SMTP server to send e-mails, and at the same time provide you with an easy to use UI to check how they are rendered.
 
_However, your developers or QA team still need to install this "dev" dependency. And because it's built on top of Node.js, they will also need to install that first._ 

Wouldn't it be easier if they could install all this automatically?

All this can be achieved with some [Docker](https://www.docker.com/ "Docker") magic.

__Prerequisites__

* Docker
* Docker Compose

__The Example__

I've created a simple project in php that sends an e-mail when you land on the index page. The working example is hosted on [github](https://github.com/tonivdv/testing-emails-during-development "github").

To run the application you only need to do the following 2 commands:

```bash
# Install app dependencies
bin/composer install

# Run the app
docker-compose up -d
```

Without any interaction from the user this will install & configure:

1. Php-fpm 7.1
1. Nginx
1. Composer
1. MailDev

Cool right :) ?

__But how does the connection work with MailDev?__

In docker you can define networks to link 2 or more containers. In docker-compose you create networks like the following:

```yaml
networks:
  backend-tier:
    driver: bridge
```

Then you 'link' that network to the relevant container. Our MailDev container is defined like the following;
 
```yaml
fakesmtp:
  image: djfarrelly/maildev
  ports:
    - 25
    - 8081:80
  networks:
    backend-tier:
      aliases:
        - my_fakesmtp
```

With this configuration it will create a MailDev container in the backend-tier network. The container will be reachable by all containers in the same network by its internal ip-address or the name of the container. As the internal ip is random, and the name (could be) too, you can provide an alias which can also be used as dns name to reach the container.

Having the php container configured in the same network:

```yaml
php:
  image: php:7.1-fpm
  expose:
      - 9000
  volumes:
      - .:/src
  networks:
      backend-tier:
        aliases:
          - my_php
```

I can now configure my dev smtp configuration of my project to `my_fakesmtp:25` and all mails will go through MailDev. 

I suggest you take a look and play with the example on [github](https://github.com/tonivdv/testing-emails-during-development "github").

__Have FUN!__