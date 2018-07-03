---
layout: post
title: Quick Guide to Update to Angular 6 and RxJS 6
subtitle: Hassle free way to update to Angular 6
category: blog
comments: true
---

**TL;DR:** In this quick guide, you will learn how to quickly update your Angular projects to [**`Angular 6`**](https://github.com/angular/angular){:target="_new"} and [**`RxJS 6`**](https://github.com/ReactiveX/rxjs){:target="_new"}. Along with litlle issues related to it.

## Introduction
Each time `Angular` rolls out newer version. It feels like a hassle to update to it. With the latest updates to `Angular` and `Angular CLI`, angular team introduced some neat ways to update projects to latest version. In this quick quide, you will learn how to update to latest version of Angular.

## Update TypeScript
In order to work with latest `@angular/cli` (v6.0.7) package. We must have TypeScript version equals to or greater than `2.7.2` and less than or equal to `2.8.0`. But the latest version of Typescript at the moment is `2.9.2`. So make sure you have the right version of TypeScript. You can install it as follows:
```bash
npm install --save-dev typescript@2.7.2
```

## Update Angular CLI
If you're still using Angular CLI version `1.0.0-beta.28` or less, first you need to uninstall the current cli as shown below:

```bash
mpm uninstall -g angular-cli
npm uninstall --save-dev angular-cli
```
And to update to the Latest CLI, run the following commands:

```bash
npm uninstall -g @angular/cli
npm cache verify
# if npm version is < 5 then use `npm cache clean`

# for global package run the following command
npm install -g @angular/cli@latest

# for local package run the following command instead
npm install --save-dev @angular/cli@latest
```

It will install the latest angular cli for your project.

## Update Angular Configuration
With Angular 6, team introduced various changes to configuration files. They also introduced `ng update` command to automatically update to latest versions.

To update the configuration of your angular project. Run the following command:
```bash
ng update @angular/cli
```

This command will do several things.
* It will generate new `angular.json` file for your project and will delete the old `.angular-cli.json` file.
* It will update `karma.conf.js`
* It will update `tslint.json` and `tsconfig.json` for your project.

## Update Angular Packages
Now, we also want to update all the Angular Packages to `6.x.x` Right? 

Following command will do it for you.
```bash
ng update @angular/core
```
Along with Angular Packages like `@angular/core` and `@angular/router` and so on, it will also update `rxjs` to latest version.

If for some reason, this command is not working. You can use `--force` flag(not recommended though)
```bash
ng update @angular/core --force
```

Also, you can update all the packages in your app, to their latest version with `--all` flag, as shown below:
```bash
ng update --all
```

## Update RxJS
`RxJS` team introduced several changes to RxJS starting with version 6. Let's briefly check these out.

### Better imports
With `RxJS 6`, you can import all the operators as follows:
```js
import { pairwise, filter, map } from 'rxjs/operators';
```

Rest of imports like `Observable`, `Observer`, `Subject` and creation methods like `of` can be imported as follows:
```js
import { Observable, Observer, Subject, of } from 'rxjs';
```

#### Lettable Operators
With `RxJS 6`, many operators are now lettable/pipeable. You can use them as follow:
```js
import { of } from 'rxjs';
import { filter } from 'rxjs/operators';

const ofObservable = of(1,2,3)
                    .pipe(filter(x => x === 1))
                    .subscribe(x => console.log(x));
```

### Migrate Automagically to RxJS 6
RxJS team also introduced migration script to ease the process.

Run the following command to install the tool.
```bash
npm i -g rxjs-tslint
rxjs-5-to-6-migrate -p [PATH_TO_TSCONFIG]
```

To use the tool, Run the following command:
```bash
rxjs-5-to-6-migrate -p [PATH_TO_TSCONFIG]
```

Replace `[PATH_TO_TSCONFIG]` with path to `tsconfig` file of your project.

It will update the imports and some other stuff. Unfortunately there are some things that you need to fix manually. So take a closer look on what code it generates.

Take a look at [Github Page](https://github.com/ReactiveX/rxjs-tslint){:target="_new"} of this package for more information.

### RxJS Compat
If for any reason, you have code written in RxJS  previous to 6. You need to install `rxjs-compat` package to your application, to have a compatibility with RxJS 6. As shown below:
```bash
npm install --save rxjs-compat
```

For more information about RxJS migration, read this piece at [LearnRxJS](https://www.learnrxjs.io/concepts/rxjs5-6.html){:target="_new"} 

## Conclusion
In this quick guide, we explored things needed to be done in order to update to *`Angular 6`* and *`RxJS`*. I hope it will help other developers.