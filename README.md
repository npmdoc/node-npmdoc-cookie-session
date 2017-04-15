# api documentation for  [cookie-session (v1.2.0)](https://github.com/expressjs/cookie-session)  [![npm package](https://img.shields.io/npm/v/npmdoc-cookie-session.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-cookie-session) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-cookie-session.svg)](https://travis-ci.org/npmdoc/node-npmdoc-cookie-session)
#### cookie session middleware

[![NPM](https://nodei.co/npm/cookie-session.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/cookie-session)

[![apidoc](https://npmdoc.github.io/node-npmdoc-cookie-session/build/screenCapture.buildCi.browser.apidoc.html.png)](https://npmdoc.github.io/node-npmdoc-cookie-session/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-cookie-session/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-cookie-session/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "bugs": {
        "url": "https://github.com/expressjs/cookie-session/issues"
    },
    "dependencies": {
        "cookies": "0.5.0",
        "debug": "~2.2.0",
        "on-headers": "~1.0.0"
    },
    "description": "cookie session middleware",
    "devDependencies": {
        "connect": "3",
        "istanbul": "0.3.17",
        "mocha": "2.2.5",
        "supertest": "1.0.1"
    },
    "directories": {},
    "dist": {
        "shasum": "9df2beb9e723998e70d1e31fda37b28a0bcf37ff",
        "tarball": "https://registry.npmjs.org/cookie-session/-/cookie-session-1.2.0.tgz"
    },
    "files": [
        "HISTORY.md",
        "LICENSE",
        "README.md",
        "index.js"
    ],
    "gitHead": "24334af3dd2da23b5cc6dc23ec75ed37a0de2bc3",
    "homepage": "https://github.com/expressjs/cookie-session",
    "keywords": [
        "connect",
        "express",
        "middleware",
        "session"
    ],
    "license": "MIT",
    "maintainers": [
        {
            "name": "jongleberry"
        },
        {
            "name": "dougwilson"
        },
        {
            "name": "tjholowaychuk"
        },
        {
            "name": "mscdex"
        },
        {
            "name": "fishrock123"
        },
        {
            "name": "defunctzombie"
        }
    ],
    "name": "cookie-session",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git+https://github.com/expressjs/cookie-session.git"
    },
    "scripts": {
        "test": "mocha --check-leaks --reporter spec --bail test/",
        "test-cov": "istanbul cover node_modules/mocha/bin/_mocha -- --check-leaks --reporter dot test/",
        "test-travis": "istanbul cover node_modules/mocha/bin/_mocha --report lcovonly -- --check-leaks --reporter spec test/"
    },
    "version": "1.2.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module cookie-session](#apidoc.module.cookie-session)
1.  [function <span class="apidocSignatureSpan"></span>cookie-session (options)](#apidoc.element.cookie-session.cookie-session)
1.  [function <span class="apidocSignatureSpan">cookie-session.</span>toString ()](#apidoc.element.cookie-session.toString)



# <a name="apidoc.module.cookie-session"></a>[module cookie-session](#apidoc.module.cookie-session)

#### <a name="apidoc.element.cookie-session.cookie-session"></a>[function <span class="apidocSignatureSpan"></span>cookie-session (options)](#apidoc.element.cookie-session.cookie-session)
- description and source-code
```javascript
function cookieSession(options) {
  var opts = options || {}

  // name - previously "opts.key"
  var name = opts.name || opts.key || 'express:sess';

  // secrets
  var keys = opts.keys;
  if (!keys && opts.secret) keys = [opts.secret];

  // defaults
  if (null == opts.overwrite) opts.overwrite = true;
  if (null == opts.httpOnly) opts.httpOnly = true;
  if (null == opts.signed) opts.signed = true;

  if (!keys && opts.signed) throw new Error('.keys required.');

  debug('session options %j', opts);

  return function _cookieSession(req, res, next) {
    var cookies = req.sessionCookies = new Cookies(req, res, keys);
    var sess, json;

    // to pass to Session()
    req.sessionOptions = Object.create(opts)
    req.sessionKey = name

    req.__defineGetter__('session', function(){
      // already retrieved
      if (sess) return sess;

      // unset
      if (false === sess) return null;

      json = cookies.get(name, req.sessionOptions)

      if (json) {
        debug('parse %s', json);
        try {
          sess = new Session(req, decode(json));
        } catch (err) {
          // backwards compatibility:
          // create a new session if parsing fails.
          // new Buffer(string, 'base64') does not seem to crash
          // when 'string' is not base64-encoded.
          // but 'JSON.parse(string)' will crash.
          if (!(err instanceof SyntaxError)) throw err;
          sess = new Session(req);
        }
      } else {
        debug('new session');
        sess = new Session(req);
      }

      return sess;
    });

    req.__defineSetter__('session', function(val){
      if (null == val) return sess = false;
      if ('object' == typeof val) return sess = new Session(req, val);
      throw new Error('req.session can only be set as null or an object.');
    });

    onHeaders(res, function setHeaders() {
      if (sess === undefined) {
        // not accessed
        return;
      }

      try {
        if (sess === false) {
          // remove
          cookies.set(name, '', req.sessionOptions)
        } else if (!json && !sess.length) {
          // do nothing if new and not populated
        } else if (sess.changed(json)) {
          // save
          sess.save();
        }
      } catch (e) {
        debug('error saving session %s', e.message);
      }
    });

    next();
  }
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.cookie-session.toString"></a>[function <span class="apidocSignatureSpan">cookie-session.</span>toString ()](#apidoc.element.cookie-session.toString)
- description and source-code
```javascript
toString = function () {
    return toString;
}
```
- example usage
```shell
...
*
* @param {String} string
* @return {Object}
* @private
*/

function decode(string) {
 var body = new Buffer(string, 'base64').toString('utf8');
 return JSON.parse(body);
}

/**
* Encode an object into a base64-encoded JSON string.
*
* @param {Object} body
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
