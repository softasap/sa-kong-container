sa-kong-container
==================

[![Build Status](https://travis-ci.org/softasap/sa-kong-container.svg?branch=master)](https://travis-ci.org/softasap/sa-kong-container)
[![License: MIT][softasap-license-image] ][softasap-license-url]
[![Ansible-Container friendly][ansible-container-image] ][ansible-container-url]

[ansible-container-image]: https://img.shields.io/badge/ansible--container-ready-brightgreen.svg
[ansible-container-url]: http://bit.ly/ansible-container
[softasap-license-image]: https://img.shields.io/badge/License-MIT-yellow.svg
[softasap-license-url]: https://opensource.org/licenses/MIT


Helper role to be executed with `ansible-container` aiming to build nginx based service for your application. Role is based on `sa-nginx` role,
your docker image might be any of ubuntu (14.04 LTS / 16.04 LTS), CentOS 7+, Fedora 25+, Alpine (3.4. 3.5 +)

Role is _best_ used in conjuntion with previously applied https://github.com/softasap/sa-container-bootstrap role.
Please check `sa-container-bootstrap` role for options.

Role can be used on  a standalone basis, including dumb-init support out of the box.

Variables
---------

User inside container used to run
```
container_user: nginx
```

Container's nginx user id
```
container_uid: 1000
```

Container init system used in base image. Can be unset (you are using smth on your own control)

```
container_init: "dumb-init" # phusion-init

dumb_init_version: "1.2.0"
```


Code in action
--------------

container.yml

we install nginx container role, and configure our application by applying some project
specific role in order to provide needed nginx tuning.


```

version: "2"
settings:
  conductor_base: alpine:3.5
  volumes:
   - temp-space:/tmp   # Used to copy static content between containers

services:
   kong:
     from: alpine:3.6
     container_name: kong
     roles:
       - {
           role: "softasap.sa-kong-container"
         }
       - {
           role: "../standalone-fallback"
         }

volumes:
  temp-space:
    docker: {}

```

See box-example for the standalone working example. It will configure application
image that will display 'OK' on connect - check it out:

[![](https://github.com/play-with-docker/stacks/raw/cff22438cb4195ace27f9b15784bbb497047afa7/assets/images/button.png)](http://play-with-docker.com?stack=https://raw.githubusercontent.com/softasap/sa-kong-container/master/box-example/docker-compose-try.yml)


Usage with sa-container-bootstrap
---------------------------------

2BD


Copyright and license
---------------------

Code licensed under the [BSD 3 clause] (https://opensource.org/licenses/BSD-3-Clause) or the [MIT License] (http://opensource.org/licenses/MIT).

Subscribe for roles updates at [FB] (https://www.facebook.com/SoftAsap/)

Join gitter discussion channel at [Gitter](https://gitter.im/softasap)
