# Zelkova

[![Clojars Project](http://clojars.org/jamesmacaulay/zelkova/latest-version.svg)](http://clojars.org/jamesmacaulay/zelkova)

[![Build Status](https://travis-ci.org/jamesmacaulay/zelkova.svg)](https://travis-ci.org/jamesmacaulay/zelkova)

[Elm](http://elm-lang.org/)-style FRP for Clojure and ClojureScript.

## Bullet points

* signal graphs are static, just like in Elm ([why?](https://www.youtube.com/watch?v=Agu6jipKfYw))
* core.async is used to wire up running graphs
* you can run as many signal graphs as you want at the same time
* graphs can share signal nodes because each signal is just a "recipe" value decoupled from the state of any running graph
* works in both Clojure and ClojureScript, but ClojureScript is currently better supported with implementations for [keyboard](https://github.com/jamesmacaulay/zelkova/blob/master/src/cljx/jamesmacaulay/zelkova/keyboard.cljx), [mouse](https://github.com/jamesmacaulay/zelkova/blob/master/src/cljx/jamesmacaulay/zelkova/mouse.cljx), and [window](https://github.com/jamesmacaulay/zelkova/blob/master/src/cljx/jamesmacaulay/zelkova/window.cljx) signals (basically straight ports from the Elm libraries)
* pairs really well with immediate-mode rendering (e.g. [React](http://facebook.github.io/react/)-based systems like [Om](https://github.com/swannodette/om) or [Reagent](http://holmsand.github.io/reagent/))
* [transducer pipelines](https://github.com/jamesmacaulay/zelkova/blob/05a5a5a4e064313424957ba9b465b36070ceb31a/test/cljx/jamesmacaulay/zelkova/signal_test.cljx#L384-L434)!!

## What does it look like?

```clojure
(ns my-app.ui
  (:require [jamesmacaulay.zelkova.signal :as z]
            [jamesmacaulay.zelkova.keyboard :as keyboard]
            [jamesmacaulay.zelkova.mouse :as mouse]))

(def saved-points-signal
  (let [shift-clicks (z/keep-when keyboard/shift mouse/clicks)]
    (->> mouse/position
         (z/sample-on shift-clicks)
         (z/reductions conj #{}))))

(def saved-points-atom
  (z/pipe-to-atom saved-points-signal))
```

## Examples

* [Todo MVC](http://jamesmacaulay.github.io/zelkova-todomvc/) ([source](https://github.com/jamesmacaulay/zelkova-todomvc/blob/master/src/cljs/zelkova_todomvc/core.cljs))
* [Mario](http://jamesmacaulay.github.io/zelkova/examples/mario/resources/public/) ([source](https://github.com/jamesmacaulay/zelkova/blob/gh-pages/examples/mario/src/mario/core.cljs))
* [Drag and Drop](http://jamesmacaulay.github.io/zelkova/examples/drag_and_drop/resources/public/) ([source](https://github.com/jamesmacaulay/zelkova/blob/gh-pages/examples/drag_and_drop/src/drag_and_drop/core.cljs))

## License

Copyright © 2014-2015

Distributed under the MIT License.
