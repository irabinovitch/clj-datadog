# clj-datadog

`clj-datadog` is a client for [Datadog](https://www.datadoghq.com) service
for the [Clojure](http://clojure.org) programming language.

[![Circle CI](https://circleci.com/gh/truckerpath/clj-datadog.svg?style=shield)](https://circleci.com/gh/truckerpath/clj-datadog)

[![Clojars Project](http://clojars.org/clj-datadog/latest-version.svg)](https://clojars.org/clj-datadog)

## Installation

Include the following dependency in your project.clj file:

    :dependencies [[clj-datadog "3.0.1"]]


## Example Usage

To start tracking events and sending them to Datadog, you firstly have
to register account and install
[DataGog agent](http://docs.datadoghq.com/guides/basic_agent_usage/)
that will start StatsD server on the machine you are running your
application.

Then import datadog in your namespace:

    (require '[truckerpath.clj-datadog.core :as dd])

Following datadog metric methods are available:

### Datadog connection

You have to provide a map with host and port of Datadog agent or an empty map
to use default (`{:host "127.0.0.1" :port 8125}`).

You may also create macros like
```clojure
(def datadog-spec {:host "ddhost" :port 8126})
(defmacro dd-inc [& args] `(dd/increment datadog-spec ~@args))
(defmacro dd-dec [& args] `(dd/decrement datadog-spec ~@args))
```

### Counters

You can use either amount or Datadog tags or both.
Decrements are completely symmetrical to increments but
with negative values.

    (dd/increment {} "page.views")
    (dd/increment {} "page.views" 10)
    (dd/increment {} "error.count" {:page "products"})
    (dd/increment {} "active.connections" 3 {:service "db"})

    (dd/decrement {} "users.online")
    (dd/decrement {} "users.online" {:group "admins"})

### Gauges

Gauges require value to be specified, but tags can be omitted

    (dd/gauge {} "total.posts" 526)
    (dd/gauge {} "total.posts" 526 {:site "main"})

### Timers

You can report time directly or using a macro that
will do reporting as a side-effect.

In second case tags are required, even if empty.

    (dd/timing {} "db.query.time" 843 {:query "find-by-id"})
    (dd/timed {} "external.service.call" {:service service}
              (http/get remote-uri {:socket-timeout timeout}))

## Component

Implementation of [component](https://github.com/stuartsierra/component) is available in `truckerpath.clj-datadog.component` package.

## Testing

To run tests do:
```bash
lein expectations
```

## License

Copyright © 2015 TruckerPath

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
