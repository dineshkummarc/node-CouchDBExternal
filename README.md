# Write CouchDB Externals. Easy.

    var CouchDBExternal = require("CouchDBExternal");
    var myExternal = CouchDBExternal("http://127.0.0.0.1:5984");

Easy.

Paul has a [great introduction to externals](http://davispj.com/2010/09/26/new-couchdb-externals-api.html).


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

      // Your turn
    }

Still easy.

### Configuring CouchDB

For CouchDB to pick up your external, you need to tell it to. Luckily
this is rather straightforward. Before we start though, we make two more
quick edits, so our externals behaves like a regular Unix* executable.

First, add this line on top of your external:

    #!/usr/bin/env node

And second, make your external executable:

    $ chmod +x myExternal # I tend to omit the .js from these, but feel
                          # free to use it for yours :)

(* Note to self: check out Windows)


#### …with Futon

See, easy. Now we can configure CouchDB. We need to add a new entry to
the configuration section `os_daemons`. There are multiple ways to do
this. One is Futon, CouchDB's built-in admin interface. Go to
`Configuration`. On the very bottom you see a link “Create New Section”.

For the section, put in `os_daemons`, for the name put in `my_external`
and for the value put in `/path/to/your/external`. Hit save and you are
done.


#### …or `curl`

You can also configure CouchDB using `curl` with a single request:

    $ curl -X PUT http://127.0.0.1:5984/_config/os_daemons/my_external \
        -d '"/path/to/your/external"'


#### …or `local.ini`

Another way is to edit your `local.ini` file directly and create a new
section in there:

    [os_damons]
    my_external = /path/to/your/external
    % if this is at the end of the file, make sure you leave a blank
    % line at the end

Then restart CouchDB. Note, the previous methods didn't require a
restart. CouchDB takes care of that for you.

#### …or `local.d` (recommended)

Even better than the one before though is to create a new file
`my_xternal.ini` with the same contents shown in the previous step and
put it into `/usr/local/etc/couchdb/local.d/`. From there, CouchDB will
pick up the settings automatically.

Then restart CouchDB. Note, the previous methods didn't require a
restart. CouchDB takes care of that for you.

Done. Yay :)


## Next

This is currently just a raw and minimal external implementation
abstraction, future features include:

 * Allow specification of `stdin` handlers.
 * Allow specification of `stdout` handlers.
 * Configurable error reporting.


## License & Copyright

(c) 2012 Jan Lehnardt <jan@apache.org>  
Licensed under the Apache License 2.0.
