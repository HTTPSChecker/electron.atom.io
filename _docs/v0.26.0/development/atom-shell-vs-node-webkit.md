---
version: v0.26.0
category: Development
title: 'Atom Shell vs Node Webkit'
source_url: 'https://github.com/electron/electron/blob/master/docs/development/atom-shell-vs-node-webkit.md'
---

# Technical differences to NW.js (formerly node-webkit)

__Note: Electron was previously named Atom Shell.__

Like NW.js, Electron provides a platform to write desktop applications
with JavaScript and HTML and has Node integration to grant access to low level
system in web pages.

But there are also fundamental differences between the two projects that make
Electron a completely separate product from NW.js:

__1. Entry of application__

In NW.js, the main entry of an application is a web page. You specify a
main page in the `package.json` and it is opened in a browser window as
the application's main window.

In Electron, the entry point is a JavaScript script. Instead of
providing a URL directly, you manually create a browser window and load
an HTML file using the API. You also need to listen to window events
to decide when to quit the application.

Electron works more like the Node.js runtime. Electron's APIs are lower level
so you can use it for browser testing in place of [PhantomJS](http://phantomjs.org/).

__2. Build system__

In order to avoid the complexity of building the whole Chromium, Electron uses
[libchromiumcontent](https://github.com/brightray/libchromiumcontent) to access
Chromium's Content API. libchromiumcontent is a single, shared library that
includes the Chromium Content module and all its dependencies. Users don't
need a powerful machine to build Electron.

__3. Node integration__

In NW.js, the Node integration in web pages requires patching Chromium to
work, while in Electron we chose a different way to integrate libuv loop with
each platform's message loop to avoid hacking Chromium. See the
[`node_bindings`](../../atom/common/) code for how that was done.

__4. Multi-context__

If you are an experienced NW.js user, you should be familiar with the
concept of Node context and web context. These concepts were invented because
of how NW.js was implemented.

By using the [multi-context](http://strongloop.com/strongblog/whats-new-node-js-v0-12-multiple-context-execution/)
feature of Node, Electron doesn't introduce a new JavaScript context in web
pages.
