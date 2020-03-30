WT - FeedHenry JavaScript SDK
========================

## Pull Requests

Be sure for each pull request that the base branch is WT and not the original FeedHenry repo.

## Version Branches

Version branches (ex. version-3.0.2) should be based off of the main develop branch. After development on the version is complete, tag the branch and merge it back into develop.

## Release Branches

To release a new version:
1. Create a branch off of the current version branch (ex. release-3.0.2).
2. Run `npm run build`.
3. Force add the entire `dist` folder. (To make this easier, remove `dist` from the .gitignore before adding files)

FeedHenry JavaScript SDK
========================
[![Build Status](https://travis-ci.org/feedhenry/fh-js-sdk.svg?branch=use-travis-ci)](https://travis-ci.org/feedhenry/fh-js-sdk)

The JavaScript SDK allows developers to integrate the FeedHenry Cloud into any web-based solution - desktop websites, mobile websites or a stand-alone JavaScript client.

The API is provided in the $fh namespace and uses a common convention for most functions, which takes the format:

    $fh.doSomething(parameterObject, successFunction, failureFunction)

Where parameterObject is an ordinary JavaScript object. The successFunction callback is called if the operation was successful and the failureFunction callback is called if the operation fails. All of these arguments are optional. If there is only one function, it is taken as the success function.

The successFunction callback is called with one argument, a result object, which is again an ordinary JavaScript object. The failureFunction callback is called with two arguments: an error code string, and an object containing additional error properties (if any).

Detailed documentation for the JavaScript SDK's API can be found here: http://docs.feedhenry.com/v3/api/app_api


## Building

The JS SDK is now built using [Browserify](http://browserify.org/).

### Development

Because of Browserify, you can write any new functions as normal node modules, and use node's "require" to load any other modules, or to be consumed by other modules.

### Testing

Write your tests in test/tests directory. Add the tests to test/browser/suite.js file and run

```
grunt test
```

This will use mocha and phatomjs to run the tests.

In fact, if your module and test don't require a browser environment, you can just run them purely in node. (You may need to add a new grunt task to run them).

To help debugging, you can run

```
grunt local
```

This will start mock servers locally and you can go to http://localhost:8200/example/index.html page to debug. You may want to run

```
grunt watch
```

In another terminal window to auto generate the combined js sdk file.

### Testling

For browser compatibility testing, we use [Testling](https://ci.testling.com/). The project page is here: https://ci.testling.com/feedhenry/fh-js-sdk.

You can also run testling locally:

```
npm install -g testling
testling
```

If testling can not find any browser locally, you either need to add browser paths to your PATH environment variable, or use

```
testling -u
```
and copy the url to a browser.

### Build

When finish developing and testing, run

```
grunt
```
To generate the release builds.

### Updating the AppForms Template App

This process should be changed, however, documenting it here.

Build the js-sdk as above.
clone the AppForms-Template repo

```
git clone git@github.com:feedhenry/AppForms-Template.git
```

switch to the feedhenry3 branch

```
git checkout feedhenry3
```

copy the appForms-backbone file generated in the js-sdk to the AppForms-Template

```
cp fh-js-sdk/dist/appForms-backbone.js AppForms-Template/client/default/lib/appform-backbone.js
```

install the node modules in the AppForms-Template

```
cd AppForms-Template
npm install
```

and run grunt

```
grunt
```

clone the app forms-project-client template app

```
git clone git://github.com/feedhenry/appforms-project-client.git
```

copy the resulting lib.min.js to the appforms template

```
cp AppForms-Template/dist/client/default/lib.min.js appforms-project-client/www/lib.min.js
```

push the updated changes

```
git push origin master
```

## Releasing

Our SDK is release on [npmjs](https://www.npmjs.com/package/fh-js-sdk). To do a release follow those steps:

### Prepare release
* Update ```package.json```, ```npm-shrinkwrap.json```, ```bower.json``` file with the new version number.
* Update ```CHANGELOG.md``` with some JIRA link
* Do a PR, make sure Travis build passed. Merge.
* Tag the repository with the new version number:

```
git tag -s -a {VERSION} -m 'version {VERSION}'   // e.g. {VERSION} format is  '2.17.0'
```

* Push the new release tag on GitHub:

```
git push origin {TAG}
```

* [Travis build](.travis.yml#L12-L18) will generate a relase for you in release tab.

### Publish to npmjs
* Login to npm as `feedhenry` or make sure your own login is part of the collaborator list in [fh-js-sdk in npmjs.org](https://www.npmjs.com/package/fh-js-sdk)

```
npm login
```

* Publish:

```
npm publish
```

### Generate API doc
The documentation is generated from `fh-js-sdk.d.ts`.
The generated doc is comitted in `doc/` folder and deployed to `gh-pages` so that feedhenry.org and portal documentation can pick it up from a published location.

To publish doc, run the command:
```
npm run doc
```
Go to [gh-pages](https://github.com/feedhenry/fh-js-sdk/tree/gh-pages) and see recent commit for the publication.

### Sync client development

Sync client development should be done in [fh-sync-js repository] (https://github.com/feedhenry/fh-sync-js)
