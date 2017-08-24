
[![Join the chat at https://gitter.im/sack-vfs/Lobby](https://badges.gitter.im/sack-vfs/gun-db.svg)](https://gitter.im/sack-vfs/gun-db?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

# gun.db
ODBC/Sqlite native persistence layer for [gun](https://github.com/amark/gun)! GUN is an Open Source Firebase with swappable storage engines (level, SQLite, etc.) that handles data synchronization across machines / devices.


Get it by

`npm install gun-db`

Use by

```javascript
var Gun = require('gun');
require('gun-db');

var gun = Gun({
  file: false // turn off pesky file.js data.json default
  , db: {
    file: "gun.db"
    exclusive : false // default
  }
});
```

If you want to have maximum speed, you can set exclusive, which will gain about 30-40% speed; but you're only allowed one instance of Gun against this database.
You can open multiple instances if they don't have the same database name.

Check the gun docs on how to read/write data, it will then handle sync automatically for you (even to the browser!). Tip: It is a graph database, so you can do key/value, document, relational, or graph based data - here is a [crash course](https://github.com/amark/gun/wiki/graphs) on how to use it.

Enjoy!

Or: Complain about bugs. :)


# notes
   If the filename is '*.db' it defaults to sqlite if it's not it tries it as a DSN (data source name) and then if that doesn't work falls back to use sqlite filename.
   ODBC can be provided by providing unixodbc on linux, but requires modifying the build to enable; it  is by default only enabled for windows.

   It also ends up writing a sql.config file somewhere ... there's options you can set there to enable sql logging (optionally with data returned) which goes to stderr
     under windows this goes to (/programdata/freedom collective/node/...) probably.  If your node.exe is not what your running it will be in a folder that is whatever the program name is minus the last (.*)
     under not windows it probably just goes to ~
     
## VFS Usage

This is an example of how to open the sqlite database in a virtual filesystem storage; the access to the sqlite database 
is then memory mapped.

```
var vfs = require( "sack.vfs" );
var vol = vfs.Volume( "MountName", "vfsFile.dat" );

var Gun = require('gun');
require('gun-db');

var gun = Gun({
  file: false // turn off pesky file.js data.json default
  , db: {
    file: "$sack@MountName$gun.db"
  }
});

/* ... your appcode ... */

```


# Changelog
- 1.0.565 disable exclusive by default; add option to enable it
- 1.0.564 test sqlite database without exclusive, but with URI filename and nolock option.
- 1.0.563 remove excess logging
- 1.0.562 store json string in data  otherwise simple number types come back as numbers and not strings (invalid graph! error)
- 1.0.561 fix last minute typo
- 1.0.56 fix writing null value and relation; fixed relation restore; remove unused code; optimize existance check
- 1.0.55 More cleanup; if database open fails, don't register handlers.
- 1.0.54 fixed typo
- 1.0.53 a little cleanup; move varibes to closure (bad debug typo)
- 1.0.52 remove noisy logging when record already up to date; post reply acking the transation.
- 1.0.51 Update docs; add gitter badge
- 1.0.4 fix excessively slow load; misported from sqlite.gun.
- 1.0.3 fix database performance options.
- 1.0.2 update to Gun 0.8.3
- 1.0.1 First usable version

