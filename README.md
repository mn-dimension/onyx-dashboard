# onyx-dashboard

** Dimension **

Additional instructions to get working on OSX March 2020. 

Why:

This repostitory will not build via `lein uberjar`. 
To resolve this we need to make sure we are using a compatible Java version; 1.8 works (more versions may be compatible)
The additon to this project over the original is to add a config file to change the java version via [jenv](https://github.com/jenv/jenv).

How:

1. Install [jenv](https://github.com/jenv/jenv) via homebrew with `brew install jenv` and follow the [instructions](https://github.com/jenv/jenv#11-installing-jenv).
2. Install java version via homebrew `brew cask install homebrew/cask-versions/adoptopenjdk8`
3. Add the versions of java to jenv `jenv add $(/usr/libexec/java_home -v 1.8) && jenv add $(/usr/libexec/java_home)`

Test:

Clone this repsitory and `cd onyx-dashboard && jenv versions` should result in version 1.8 having a * next to it.
The code will now build via `lein uberjar`

Run:

`java -jar target/onyx-dashboard.jar localhost:2181 1` where zookeeper is listening localy with default port 2181.

**UNMAINTAINED**

A dashboard for the [Onyx](https://github.com/onyx-platform/onyx) distributed computation system.

<img src="doc/screenshot.jpg" width="400">

[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/onyx-platform/onyx?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![Circle CI](https://circleci.com/gh/onyx-platform/onyx-dashboard.svg?style=svg)](https://circleci.com/gh/onyx-platform/onyx-dashboard)

## Design and User Guide

A description and user guide disguised as a blog post can be found [here](http://lbradstreet.github.io/clojure/onyx/distributed-systems/2015/02/18/onyx-dashboard.html).

## Versioning

Version numbers will be kept in sync with [Onyx]
(https://github.com/onyx-platform/onyx). For example, to use a version
compatible with Onyx `0.8.1.0-alpha8`, use a version of the dashboard beginning with
`0.8.1.0-alpha8`. The fourth version number will be reserved for dashboard versioning, in
order to provide releases out of band with Onyx.

Releases are listed at [RELEASES.md](RELEASES.md).

## Deployment

Run the jar via:
```
java -server -jar onyx-dashboard-VERSION.jar ZOOKEEPER_ADDR
```

By default the server will listen on port 3000, but this can be configured via the PORT environment variable.

Alternately, run the docker image like so:
```
docker run -p 3000:3000 onyx/onyx-dashboard:<tag> 192.168.1.170:2188
```

The IP passed in is used by ZooKeeper.

NOTE: If you are running the Onyx Dashboard via Docker on Mac, but are running ZooKeeper locally, be aware
that there are limitations with networking on Mac. Per the [Docker on Mac networking documentation](https://docs.docker.com/docker-for-mac/networking/),
your best work around is to assign a dummy IP as an alias to the loopback adapter on the host machine
and point the Docker image at that IP. Something like this should do the trick:
```
sudo ifconfig lo0 alias 10.200.10.1/24
docker run -p 3000:3000 onyx/onyx-dashboard:latest 10.200.10.1:2188
```

## Development

In a terminal start a sample job and cluster to dashboard against:
```
lein run -m onyx.peer.dag-test
```

Then start developyment:

Run `lein repl` to start your repl.

In the REPL, run
```clojure
(onyx-dashboard.dev/start-figwheel)
```

Then run:

```clojure
(user/stop) (clojure.tools.namespace.repl/refresh) (user/go "127.0.0.1:2188")
```
to start the server, and each time you make a change on the server.

Then point your browser at http://localhost:3000/

## License

Copyright © 2016 Distributed Masonry

Distributed under the Eclipse Public License either version 1.0 or (at your option) any later version.

## Chestnut

Created with [Chestnut](http://plexus.github.io/chestnut/) 0.7.0-SNAPSHOT (ecadc3ce).
