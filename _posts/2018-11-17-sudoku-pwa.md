---
layout: post
title:  "PWA Angular Sudoku"
date:   2018-11-17 14:34:11 +0100
categories: angular sudoku pwa
comments: true
---

It's pretty simple adding [PWA](https://developers.google.com/web/progressive-web-apps/) capability to a simple application like Sudoku.

Run the command below in order to apply dependencies and basic configuration to be compliant with PWA requirements:
```
ng add @angular/pwa@v6-lts
```
Beware that the version `v6-lts` is needed in case you are using Angular 6 / Node 8.

Configure settings in `manifest.json`, such as application name, scope, url and icons; double check that `start_url` is matching with the starting page configured in angular's routing.

Build and deploy the production version and check that everything is ok using `Audits` and `Application` tools available from Chrome dev tools.

If everything is ok, _Install "application name".._ will appear in Desktop Chrome's menu and _Add to homescreen_ will be proposed opening it with Android devices.
