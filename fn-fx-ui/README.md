# fn-fx-ui

A spike with `fn-fx` to build a simple GUI with Clojure and JavaFX.

## Issues
- Installed `fn-fx` locally because the latest versions are not on Clojars
- Order of `requires` is important to get JavaFX initialize correctly and prevent issues with
threads

### Starting app from Leiningen [SOLVED]
Had trouble to get `lein run` or Uberjar to work, program exits immediately after rendering
the UI, fixed via `(:gen-class :extends javafx.application.Application)`

### Modal Dialog [SOLVED]
Struggling to show a Dialog with the `fn-fx` library at this needs to be run from the event
thread but I don't know how to get it there, might also be an JavaFX lifecycle issue as
indicated by
[this SO answer](https://stackoverflow.com/questions/33966259/javafx-thread-issues/34005514#34005514).

Solved it by using the function `run-and-wait` to show the dialog.

### Diffing fails in some cases [OPEN]

When updating the table data the diffing to rerender the UI can fail because of an not yet
implemented function:

```
#error {
 :cause Assert failed: TODO: Implement this
(= idx (dec (count lst)))
 :via
 [{:type java.lang.AssertionError
   :message Assert failed: TODO: Implement this
(= idx (dec (count lst)))
   :at [fn_fx.fx_dom.FXDom$fn__971 invoke fx_dom.clj 39]}]
 :trace
 [[fn_fx.fx_dom.FXDom$fn__971 invoke fx_dom.clj 39]
  [clojure.lang.AFn run AFn.java 22]
  [com.sun.javafx.application.PlatformImpl lambda$null$173 PlatformImpl.java 295]
  [java.security.AccessController doPrivileged AccessController.java -2]
  [com.sun.javafx.application.PlatformImpl lambda$runLater$174 PlatformImpl.java 294]
  [com.sun.glass.ui.InvokeLaterDispatcher$Future run InvokeLaterDispatcher.java 95]]}
#error {
 :cause Assert failed: TODO: Implement this
(= idx (dec (count lst)))
 :via
 [{:type java.lang.AssertionError
   :message Assert failed: TODO: Implement this
(= idx (dec (count lst)))
   :at [fn_fx.fx_dom.FXDom$fn__971 invoke fx_dom.clj 39]}]
 :trace
 [[fn_fx.fx_dom.FXDom$fn__971 invoke fx_dom.clj 39]
  [clojure.lang.AFn run AFn.java 22]
  [com.sun.javafx.application.PlatformImpl lambda$null$173 PlatformImpl.java 295]
  [java.security.AccessController doPrivileged AccessController.java -2]
  [com.sun.javafx.application.PlatformImpl lambda$runLater$174 PlatformImpl.java 294]
  [com.sun.glass.ui.InvokeLaterDispatcher$Future run InvokeLaterDispatcher.java 95]]}
```


### Constructing components that have a constructor with mandatory arguments [SOLVED]

For example ScatterPlot needs to have two axes in the constructor. Just using
`controls/scatter-plot` seems not to allow passing those. `render-core/construct-control` seems to
be something that might get used to do that.

To trigger that, one would need to call `create-component!` not with a keyword `type` but a vector
`[tp args]`.

I was able to build the component myself via `diff/component` providing the class as keyword, an
empty vector of `arg-names` (because the constructor has empty `prop-names-kw`) and the mandatory
axes. This seems to be a bit brittle, because there is another constructor which has empty
`prop-names-kw` but needs a third argument, the data to be plotted. At some point this constructor
might be chosen by `diff/component` instead of the one we need.

## License

Copyright © 2017 Dr. Nils Blum-Oeste

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
