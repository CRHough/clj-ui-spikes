# fn-fx-ui

A spike with `fn-fx` to build a simple GUI with Clojure and JavaFX.

## Issues
- Installed `fn-fx` locally because the latest versions are not on Clojars
- Order of `requires` is important to get JavaFX initialize correctly and prevent issues with
threads
- Had trouble to get `lein run` or Uberjar to work, program exits immediately after rendering
the UI, fixed via `(:gen-class :extends javafx.application.Application)`
- Struggling to show a Dialog with the `fn-fx` library at this needs to be run from the event
thread but I don't know how to get it there, might also be an JavaFX lifecycle issue as
indicated by [[https://stackoverflow.com/questions/33966259/javafx-thread-issues/34005514#34005514][this SO answer]]

## License

Copyright © 2017 Dr. Nils Blum-Oeste

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
