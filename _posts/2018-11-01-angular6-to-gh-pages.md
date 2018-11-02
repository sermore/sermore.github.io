---
layout: post
title:  "How to publish Angular 6 applications to Github pages"
date:   2018-11-01 20:28:11 +0100
categories: angular gh-pages github
comments: true
---

A quick guide to publish Angular 6 applications using Github project pages.

First, you have to install the package `angular-cli-pages` with yarn
```
yarn global add angular-cli-ghpages
```
or npm
```
npm install -g angular-cli-ghpages
```

Then, you have to build the production version of the application
```
ng build --prod --base-href "https://<github-user>.github.io/<project-name>/"
```

As last step, you have to deploy the application to github pages, checking that the folder created by the build process inside the `dist` folder is correctly listed in the `--dist` parameter
```
ngh --dist=dist/<project-name>
```

That's all.
