---
id: performance
title: Performance
---


HTTP Caching
------------

HTTP caching for assets (e.g. images, stylesheets or Javascript components) must be defined in the configuration of the web server.

If you are using the configuration for Apache2 or NGINX described in the [Server Setup](server-setup.md#webserver) section, the recommended configuration is already in place and no further adjustment is necessary. 

If not, all files of the following directories that are delivered directly by the web server should be delivered with a Cache HTTP Header.

- /static
- /uploads
- /themes
- /assets

:::tip
The expiration time can be set very high, because HumHub automatically changes the URL in case of an update.
:::

Application Caching
-------------------

HumHub supports different caching systems which can be configured at: `Administration -> Settings -> Advanced -> Caching`.

In addition to those listed caching systems, you can use any Yii2 compatible caching driver.

**[Configuration file](advanced-configuration.md) examples:**

Redis Cache Example:

```php
[
    'components' => [
        'cache' => [
            'class' => 'yii\redis\Cache',
            'redis' => [
                'hostname' => 'localhost',
                'port' => 6379,
                'database' => 0,
            ]
        ],
    ],
]
``` 

Memcached Configuration Example:

```php
[
    'components' => [
        'cache' => [
            'class' => 'yii\caching\MemCache',
            'servers' => [
                [
                    'host' => 'server1',
                    'port' => 11211,
                    'weight' => 60,
                ],
                [
                    'host' => 'server2',
                    'port' => 11211,
                    'weight' => 40,
                ],
            ],
        ],
    ],
]
```



Image Processing
----------------

We recommend using the `imagemagick` commandline tool for faster image processing (resizing, file format conversions).

You can activate imagemagick at:  `Administration -> Settings -> Advanced -> Files` by entering the location of the `convert` command (e.g. /usr/bin/convert). 


Job Scheduling
--------------

TBD


X-Sendfile
----------

X-Sendfile is a feature that allows us to pass file download requests directly by the webserver.
This improves the application performance.

**Installation**
Administration -> Settings -> Files -> Enable X-Sendfile Support.

Apache Config Example:

```        
XSendFile On
XSendFilePath /path/to/humhub/uploads
```

**More Information:**

- Apache: [X-Sendfile](http://tn123.org/mod_xsendfile)
- Lighttpd v1.4: [X-LIGHTTPD-send-file](http://redmine.lighttpd.net/projects/lighttpd/wiki/X-LIGHTTPD-send-file)
- Lighttpd v1.5: [X-Sendfile](http://redmine.lighttpd.net/projects/lighttpd/wiki/X-LIGHTTPD-send-file)
- Nginx: [X-Accel-Redirect](http://wiki.nginx.org/XSendfile)
- Cherokee: [X-Sendfile and X-Accel-Redirect](http://www.cherokee-project.com/doc/other_goodies.html#x-sendfile)

Requires package `libapache2-mod-xsendfile`

 