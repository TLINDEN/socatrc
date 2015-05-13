## socatrc - socat init.d wrapper

This is the README file for socatrc.

Socatrc is a small perl script which can be used to start
(and stop) multiple instances of socat via init.d.

## Documentation

Usage:

```
Usage: socatrc [-dscvh] <mode>

Options:
 --daemon | -d <path>    location of daemon program
 --socat  | -s <path>    location of socat program
 --piddir | -p <path>    location for pidfile storage
 --config | -c <path>    location of config
 --help   | -h | -?      print usage
 --version| -v           print version

Mode might be one of: start,stop,status or restart.
```

Socatrc uses the FreeBSD 'daemon(8)' tool to start socat processes into
the background. Therefore it can only be used for socat listeners.

Processes must be configured via an config file in ini format. Example:

```
piddir = /tmp
daemon = /usr/sbin/daemon
socat  = /usr/local/bin/socat
fork   = fork,reuseaddr

[wwwfwd]
        args   = -ly
        listen = TCP4-LISTEN:9999,${fork}
        sendto = TCP4:www.w3c.org:www

[dnsfwd]
        args   = -ly
        listen = UDP4-LISTEN:53,${fork}
        sendto = UDP4:8.8.8.8:53
```

The syntax is pretty self explanatory. The socat commandline will
be constructed of:

```
socat $args $listen $sendto
```

You can define global variables and use them inside process definitions.
There might be as multiple processes as you want.

To start the socat processes run:

```
socatrc start
```

And to stop:

```
socatrc stop
```

You might put this into an init script.

## Installation

Just copy 'socat' to whereever you want. Perl is required, no additional
perl modules are required though.

## Getting help

Although I'm happy to hear from socatrc users in private email,
that's the best way for me to forget to do something.

In order to report a bug, unexpected behavior, feature requests
or to submit a patch, please open an issue on github:
https://github.com/TLINDEN/socatrc/issues.

## Copyright and license

This software is licensed under the Perl Artistic License 2.0

## Authors

T.v.Dein <tom AT vondein DOT org>

## Project homepage

https://github.com/TLINDEN/socatrc
