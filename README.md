keepassrsp.js
==========

[![License](http://img.shields.io/badge/license-GPLv3-blue.svg)](https://github.com/NeoXiD/keepassrsp.js/blob/master/LICENSE.md)
[![Build Status](http://img.shields.io/travis/NeoXiD/keepassrsp.js/develop.svg)](http://travis-ci.org/NeoXiD/keepassrsp.js)
[![Dependency Status](http://img.shields.io/david/NeoXiD/keepassrsp.js.svg)](https://david-dm.org/NeoXiD/keepassrsp.js)
[![Coverage Status](http://img.shields.io/coveralls/NeoXiD/keepassrsp.js/develop.svg)](https://coveralls.io/r/NeoXiD/keepassrsp.js?branch=develop)
[![NPM version](http://img.shields.io/npm/v/keepassrsp.js.svg)](https://npmjs.org/package/keepassrsp.js)
[![Gittip](http://img.shields.io/gittip/NeoXiD.svg)](https://www.gittip.com/NeoXiD)


*keepassrsp.js* is a Node.js library for reading and writing KeePass databases. Please note that currently only the newest database version, called KDBX, is supported. Features include so far:

- **Password and/or Keyfile credentials**: keepassrsp.js supports both of the most common used credential types for KeePass databases.
- **Powerful API**: This library offers you a powerful API, which even allows you raw access to the database, so even unsupported third-party fields can be modified.
- **Joyfully simple and flexible**: I've built keepassrsp.js to be easily understandable and a joy to use. It's built with JavaScript and tries to provide a solid foundation for modifying KDBX databases.
- **Stunning performance**: To further improve performance, keepassrsp.js even includes an optional native library, which will help while performing the key transformations. If your system should not have the *Crypto++ Dev Libraries* installed, it will automatically fallback to the slower Node.js methods.

-
**Note**: *keepassrsp.js is currently under active development. As such, while this library is well-tested, the API might change at anytime. Consider using it in production applications only if you're comfortable following a changelog and updating your usage accordingly.*


Example
-------
As mentioned above, keepassrsp.js is really easy to use. The following example code opens a database, outputs its name, changes the name to *'KeePass.IO rocks!'* and saves the database with new credentials. More examples are available within the *examples* folder.

```javascript
var path = require('path');
var kpio = require('../lib');

var db = new kpio.Database();
db.addCredential(new kpio.Credentials.Password('thematrix'));
db.addCredential(new kpio.Credentials.Keyfile('apoc.key'));
db.loadFile(databasePath, function(err) {
	if(err) throw err;

	var rawDatabase = db.getRawApi().get();
	console.log('Database name: ' + rawDatabase.KeePassFile.Meta.DatabaseName);
	rawDatabase.KeePassFile.Meta.DatabaseName = 'KeePass.IO rocks!';

	db.resetCredentials();
	db.addCredential(new kpio.Credentials.Password('morpheus'));
	db.addCredential(new kpio.Credentials.Keyfile('trinity.key'));

	db.getRawApi().set(rawDatabase);
	db.saveFile(newDatabasePath, function(err) {
		if(err) throw err;
	});
});
```


Copyright
---------
Copyright &copy; 2013-2014 Pascal Mathis. All rights reserved.