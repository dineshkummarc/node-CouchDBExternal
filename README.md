# Write CouchDB Externals. Easy.

    var CouchDBExternal = require("CouchDBExternal");
    var myExternal = CouchDBExternal("http://127.0.0.0.1:5984");

Easy.

## Wha?

All CouchDBExternal does is waiting for CouchDB to launch it. We'll show
how that works in a second. After launching, CouchDBExternal open
`stdin` and hangs in there for any commands that might come through.

For now, consider this an interface, you can use it standalone, but it
won't do you much good as is. But it is easy to extend for your own
purposes. For a full example see
<https://github.com/janl/node-couchdb-external-CreateUserDatabase>.

Here's a short example:

    var CouchDBExternal = require("CouchDBExternal");
    util.inherits(MyCouchDBExternal, CouchDBExternal);

    function MyCouchDBExternal() {
      var server = "http://127.0.0.1:5984";
      CouchDBExternal.call(this, server);

      // Your Turn
    }


## Next

This is currently just a raw and minimal external implementation
abstraction, future features include:

 * Allow specification of `stdin` handlers.
 * Allow specification of `stdout` handlers.
 * Configurable error reporting.


## License & Copyright

(c) 2012 Jan Lehnardt <jan@apache.org>  
Licensed under the Apache License 2.0.
