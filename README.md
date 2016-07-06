# Angular2-Meteor [![NPM version][npm-version-image]][npm-url] [![NPM downloads][npm-downloads-image]][npm-url] [![Build Status](https://travis-ci.org/Urigo/angular2-meteor.svg?branch=master)](https://travis-ci.org/Urigo/angular2-meteor)  [![Join the chat at https://gitter.im/Reactive-Extensions/RxJS](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/Urigo/angular-meteor)
Angular2 + Meteor integration.

## Table of Contents
* [Tutorial](../../#tutorial)
* [Quick Start](../../#quick-start)
    * [Install package](../../#install-package)
    * [Using Meteor in your angular2 app](../../#using-meteor-in-your-angular2-app)
* [Demos](../../#demos)
* [TypeScript](../../#typescript)
* [Roadmap](../../#roadmap)
* [Contribution](../../#contribution)

## Tutorial

If you are new to Angular2 and/or Meteor but want to learn them quickly, 
please check out our 23-steps Angular2-Meteor [tutorial](http://www.angular-meteor.com/tutorials/socially/angular2/bootstrapping).

## Questions and Issues

If you have rather a question than an issue, please consider the following resources at first:
- [Gitter](https://gitter.im/Urigo/angular-meteor)
- [Stack Overflow `angular2-meteor` tag](http://stackoverflow.com/questions/tagged/angular2-meteor)
- [Meteor forum](https://forums.meteor.com/c/angular)

The chances to get a quick response there is higher than posting a new issue here.

If you've decided that it's likely a real issue, please consider going through the following list at first:
- Check quickly recently [created](https://github.com/Urigo/angular2-meteor/issues)/[closed](https://github.com/Urigo/angular2-meteor/issues?q=is%3Aissue+is%3Aclosed) issues: chances are high that someone's already created a similar one
  or similar issue's been resolved;
- If your issue looks nontrivial, we would approciate a small demo to reproduce it.
  You will also get a response much faster.

## Quick Start

### Install package:

To install Angular2-Meteor's NPMs:
````
    npm install angular2-meteor --save
    npm install angular2-meteor-auto-bootstrap --save
````

Second step is to add ` angular2-compilers` package — `meteor add angular2-compilers`.
This package adds custom HTML processor, LESS and TypeScript compilers.
Custom HTML processor and LESS compiler make sure that static resources are used
in the way that Angular 2 requires, and TypeScript is a recommended JS-superset to develop with Angular 2.

Please note that you'll have to remove the standard Meteor HTML processor (and LESS package).
The reason is that Meteor doesn't allow more than two processor for one extension:
````
   meteor remove blaze-html-templates
````

Angular 2 heavily relies on some polyfills (`zone.js`, `reflect-metadata` etc).
There are two ways to add them:
- Add `import 'angular2-meteor-polyfills'` in every file that needs polyfills;
- Add `barbatus:angular2-polyfills` package. Since it's a package, it's loaded by Meteor before any user code.

Please, don't forget to add a main HTML file (can be `index.html` or with any other name) even if your app template consists of one single tag,
e.g., `<body><app></app></body>`.

### Using Meteor in your Angular 2 app:

This package contains modules that simplify development of a Meteor app with Angular 2.

You can use Meteor collections in the same way as you would do in a regular Meteor app with Blaze,
the only thing required is to make use of the `bootstrap` method from `angular2-meteor-auto-bootstrap`:

````js
import {bootstrap} from 'angular2-meteor-auto-bootstrap';
````

And now you can iterate `Mongo.Cursor` objects with Angular 2.0 ngFor!

For example, change `client/app.ts` to:
````ts
    // ....

    @Component({
      templateUrl: 'client/parties.html'
    })
    class Socially {
        constructor() {
          this.parties = Parties.find();
        }
    }

    // ....
````

Add Angular2 template file `client/parties.html` with a content as follows:
````html
    <div *ngFor="let party of parties">
      <p>Name: {{party.name}}</p>
    </div>
````

At this moment, you are ready to create awesome apps backed by the power of Angular 2 and Meteor!

Other one feature is to extend a component with `MeteorComponent`. `MeteorComponent` wraps major Meteor
methods and does some work behind the scene (such as cleanup) for you to make sure your component
stays ok:

````ts
    import {bootstrap} from 'angular2-meteor-auto-bootstrap';
    import {MeteorComponent} from 'angular2-meteor';
    import {MyCollection} form '../model/my-collection.ts';

    @Component({
      selector: 'socially'
      template: "<p>Hello World!</p>"
    })
    class Socially extends MeteorComponent {
      myData : Mongo.Cursor<any>;
    
      constructor() {
         this.myData = MyCollection.find({});
         this.subscribe('my-subscription'); // Wraps Meteor.subscribe
      }
      
      doSomething() {
         this.call('server-method'); // Wraps Meteor.call
      }
    }

    bootstrap(Socially);
````
You can read more about `MeteorComponent` in the [tutorial section] (http://www.angular-meteor.com/tutorials/socially/angular2/privacy-and-publish-subscribe-functions)!

## Demos

Check out two demos for the quick how-to:

* the Tutorial's [Socially app](https://github.com/Urigo/meteor-angular2.0-socially)
* Simple [Todos](https://github.com/Urigo/Meteor-Angular2/tree/master/examples/todos-meteor-1.3) demo

## TypeScript
The package uses [TypeScript for Meteor](https://github.com/barbatus/typescript) to compile (transpile) `.ts`-files.

TypeScript configuration file a.k.a. `tsconfig.json` is supported as well.

Please note that `tsconfig.json` is not required, but if you want to configure TypeScript
in your IDE or add more options, place `tsconfig.json` in the root folder of your app.
You can read about all available compiler options [here](https://github.com/Microsoft/TypeScript/wiki/tsconfig.json).

Default TypeScript options for Meteor 1.3 are as follows:
````json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "module": "commonjs",
    "target": "es5",
    "moduleResolution": "node",
    "emitDecoratorMetadata": true,
    "sourceMap": true
  }
}
````

### Typings

To add declaration files of any global 3-party JavaScript library including Meteor itself (so called ambient typings), we recommend to use the [`typings`](https://github.com/typings/typings) utility, which is designed to search across and install typings from `DefinitelyTyped` and own typings registries.

For example, to install Meteor declaration file run:
````
npm install typings -g

typings install registry:env/meteor --global
````

For more information on Meteor typings, please read [here](https://github.com/meteor-typings/meteor).

Please note that you don't need to worry about Angular 2's typings and typings of the related NPMs!
TypeScript finds and checkes them in NPMs automatically.

## Change Log

Change log of the package is located [here](CHANGELOG.md).

## Roadmap

You can check out the package's roadmap and its status under this repository's issues.

## Contribution
If you know how to make integration of Angular 2 and Meteor better, you are very welcome!

Check out [CONTRIBUTION.md](CONTRIBUTION.md) for more info.

[npm-downloads-image]: http://img.shields.io/npm/dm/angular2-meteor.svg?style=flat
[npm-version-image]: http://img.shields.io/npm/v/angular2-meteor.svg?style=flat
[npm-url]: https://npmjs.org/package/angular2-meteor
