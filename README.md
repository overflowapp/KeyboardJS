This is a fork of [KeyboardJS](https://github.com/sladwig/KeyboardJS) which is a fork of [KeyboardJS](https://github.com/RobertWHurst/KeyboardJS) (original Readme below), since it seems a bit inactive. This fork adds a tiny change to better handle international keyboard layouts, by also looking at the `.key` value of a keyboard event.

I also added types from [@types/keyboardjs](https://www.npmjs.com/package/@types/keyboardjs).

To replace the current KeyboardJS implementation without having to rewrite all the `imports` throughout your codebase, you can use the tarball url in `package.json`:

```
{
    "dependencies": {
        ...
        "keyboardjs": "https://registry.npmjs.org/@protoio-labs/keyboardjs/-/keyboardjs-2.6.1.tgz",
        ...
    }
}
```

Use `npm view [package name] dist.tarball` to get the tarball of any npm package.

Adding `resolutions` or using specific versions for KeyboardJS in your`package.json`, might help migrating to the new version.

_original (maybe adapted) Readme below_

---

# KeyboardJS

[...removed original banners, since they are not representing this repository...]

KeyboardJS is a library for use in the browser (node.js compatible). It Allows
developers to easily setup key bindings. Use key combos to setup complex
bindings. KeyboardJS also provides contexts. Contexts are great for single page
applications. They allow you to scope your bindings to various parts of your
application. Out of the box keyboardJS uses a US keyboard locale. If you need
support for a different type of keyboard KeyboardJS provides custom locale
support so you can create with a locale that better matches your needs.

KeyboardJS is available as a NPM module for use with
[browserify](http://browserify.org/) (or in node.js). If you don't use
browserify you can simply include
[keyboard.js](https://github.com/RobertWHurst/KeyboardJS/blob/master/dist/keyboard.js)
or
[keyboard.min.js](https://github.com/RobertWHurst/KeyboardJS/blob/master/dist/keyboard.min.js)
from the dist folder in this repo. These files are
[UMD](https://github.com/umdjs/umd) wrapped so they can be used with or without
a module loader such as [requireJS](http://requirejs.org/).

```shell
npm install @protoio-labs/keyboardjs
```

Note that all key names can be found in [./locales/us.js](https://github.com/overflowapp/KeyboardJS/blob/master/locales/us.js).

If you're looking for the previous v1.x.x release of KeyboardJS you can find it
[here](https://github.com/RobertWHurst/KeyboardJS/tree/legacy).

**Setting up bindings is easy**

```javascript
keyboardJS.bind("a", function (e) {
  console.log("a is pressed");
});
keyboardJS.bind("a + b", function (e) {
  console.log("a and b is pressed");
});
keyboardJS.bind("a + b > c", function (e) {
  console.log("a and b then c is pressed");
});
keyboardJS.bind(["a + b > c", "z + y > z"], function (e) {
  console.log("a and b then c or z and y then z is pressed");
});
keyboardJS.bind("", function (e) {
  console.log("any key was pressed");
});
//alt, shift, ctrl, etc must be lowercase
keyboardJS.bind("alt+shift+a", function (e) {
  console.log("alt, shift and a is pressed");
});

// keyboardJS.bind === keyboardJS.on === keyboardJS.addListener
```

**keydown vs a keyup**

```javascript
keyboardJS.bind(
  "a",
  function (e) {
    console.log("a is pressed");
  },
  function (e) {
    console.log("a is released");
  }
);
keyboardJS.bind("a", null, function (e) {
  console.log("a is released");
});
```

**Prevent keydown repeat**

```javascript
keyboardJS.bind("a", function (e) {
  // this function will once run once even if a is held
  e.preventRepeat();
  console.log("a is pressed");
});
```

**Unbind things**

```javascript
keyboardJS.unbind("a", previouslyBoundHandler);
// keyboardJS.unbind === keyboardJS.off === keyboardJS.removeListener
```

**Using contexts**

```javascript
// these will execute in all contexts
keyboardJS.bind("a", function (e) {});
keyboardJS.bind("b", function (e) {});
keyboardJS.bind("c", function (e) {});

// these will execute in the index context
keyboardJS.setContext("index");
keyboardJS.bind("1", function (e) {});
keyboardJS.bind("2", function (e) {});
keyboardJS.bind("3", function (e) {});

// these will execute in the foo context
keyboardJS.setContext("foo");
keyboardJS.bind("x", function (e) {});
keyboardJS.bind("y", function (e) {});
keyboardJS.bind("z", function (e) {});

// if we have a router we can activate these contexts when appropriate
myRouter.on("GET /", function (e) {
  keyboardJS.setContext("index");
});
myRouter.on("GET /foo", function (e) {
  keyboardJS.setContext("foo");
});

// you can always figure out your context too
var contextName = keyboardJS.getContext();

// you can also set up handlers for a context without losing the current context
keyboardJS.withContext("bar", function () {
  // these will execute in the bar context
  keyboardJS.bind("7", function (e) {});
  keyboardJS.bind("8", function (e) {});
  keyboardJS.bind("9", function (e) {});
});
```

**pause, resume, and reset**

```javascript
// the keyboard will no longer trigger bindings
keyboardJS.pause();

// the keyboard will once again trigger bindings
keyboardJS.resume();

// all active bindings will released and unbound,
// pressed keys will be cleared
keyboardJS.reset();
```

**pressKey, releaseKey, and releaseAllKeys**

```javascript
// pressKey
keyboardJS.pressKey("a");
// or
keyboardJS.pressKey(65);

// releaseKey
keyboardJS.releaseKey("a");
// or
keyboardJS.releaseKey(65);

// releaseAllKeys
keyboardJS.releaseAllKeys();
```

**watch and stop**

```javascript
// bind to the window and document in the current window
keyboardJS.watch();

// or pass your own window and document
keyboardJS.watch(myDoc);
keyboardJS.watch(myWin, myDoc);

// or scope to a specific element
keyboardJS.watch(myForm);
keyboardJS.watch(myWin, myForm);

// detach KeyboardJS from the window and document/element
keyboardJS.stop();
```
