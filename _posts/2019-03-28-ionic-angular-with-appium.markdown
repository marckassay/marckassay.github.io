---
layout: post
title:  "Ionic 4 Angular with Appium"
date:   2019-03-28 11:48:16 -0400
categories: test automation
---

If you're familiar with test automation using Appium, the title of this post might have you already considering on how a hybrid app is going to be successfully tested, as there are several factors to consider. Prior to publishing this article, my tribulations of completing a successful test "spec" was such that I felt an article, hopefully this one, will remove some uncertainties for the next developer or technician facing the same objectives.

This was my second experience with this sort of test automation, that both required tremendous amount of trial-and-error. From the documentation that I read online seem to frequently conflict with what I experienced when implemented in my IDE. With implementation frameworks with the same name as the specification only added to my confusion when trying to visualize a hierarchy of constituents (i.e. frameworks, protocols and selector-strategies). This test automation landscape is vast with several layers of abstraction between these constituents. That said, and until more is known, my objective for this article is to simply guide you to pass just a couple of simple test specs without creating more uncertainties for you. Those test specs involve a few swipe and tap gesture followed by an assertion to verify expected data is indeed shown. If you are able to do that I'm hoping that will clear the fog to develop your own basic heuristics for additional specs. So let me explain, to the best of my knowledge, the landscape of significant constituents that will be used in this setup of "Ionic 4 Angular with Appium".

## Constituents

If you do not become familiar with the constituents, such as the ones listed below, you're likely going to limit yourself to a deeper understanding and perhaps an appreciation to what has been developed. But if you want to prove to yourself at this moment that you can complete a successful test, then clone (or fork) ['Ionic4 Angular With Appium'](https://github.com/marckassay/Ionic4AngularWithAppium) and skip to the tutorial 'Start AUT Process' section. Afterwards, recollect your thoughts and consider reading this section.

So here is a list of some constituents in this concoction of technologies we are about to use. Not all are used directly in this tutorial, but some fall in the realm where it's important to be aware of them since mentioned by other resources online. Each has a description from the organization that has ownership of the product, followed by a comment of my own on how it's related to this tutorial:

### UI Frameworks

#### Ionic

> "... is the free, open source mobile UI toolkit for developing high-quality cross-platform apps for native iOS, Android, and the web—all from a single codebase." - [ionicframework.com](https://ionicframework.com/)

This is our "master link" in the tool chain for the app's source code that utilizes Cordova for cross compiling, which will output the APK file.

<br/>

#### Angular

> "Learn one way to build applications with Angular and reuse your code and abilities to build apps for any deployment target. For web, mobile web, native mobile and native desktop." - [angular.io](https://angular.io/)

The chosen JavaScript framework for the app. An alternative framework could of been "[Vue](https://vuejs.org/), [React](https://reactjs.org/), [Preact](https://preactjs.com/), etc"

<br/>

### Automation Servers

#### Appium

> "Appium is an open source test automation framework for use with native, hybrid and mobile web apps.
>
> It drives iOS, Android, and Windows apps using the WebDriver protocol." - [appium.io](http://appium.io/)

The test automation framework that will drive our WebdriverIO requests. It's not limited to WebdriverIO requests, but can also drive "... Selenium WebDriver."<sup>[1](http://www.mindfiresolutions.com/blog/2018/05/selenium-vs-appium/)</sup>

<br/>

#### Selenium

> "Selenium automates browsers. That's it! What you do with that power is entirely up to you. Primarily, it is for automating web applications for testing purposes, but is certainly not limited to just that. Boring web-based administration tasks can (and should!) be automated as well.
>
> Selenium has the support of some of the largest browser vendors who have taken (or are taking) steps to make Selenium a native part of their browser. It is also the core technology in countless other browser automation tools, APIs and frameworks." - [seleniumhq.org](https://www.seleniumhq.org/)

Although not used in this tutorial, "...evaluate various websites and web applications running on desktop browsers."<sup>[2](http://www.mindfiresolutions.com/blog/2018/05/selenium-vs-appium/)</sup>

<br/>

#### Karma

> "Karma is essentially a tool which spawns a web server that executes source code against test code for each of the browsers connected. The results of each test against each browser are examined and displayed via the command line to the developer such that they can see which browsers and tests passed or failed." - [karma-runner.github.io/.../how-it-works.html](https://karma-runner.github.io/3.0/intro/how-it-works.html)

Although not used in this tutorial, it's included when an Angular project is created. I'm not completely familiar with Karma, but this seems to be a unit test runner and watcher that can run locally while a developer makes edits.

<br/>

### Drivers

#### WebDriver (the W3C specification, the successor to JSON Wire Protocol)

> "WebDriver is a remote control interface that enables introspection and control of user agents. It provides a platform- and language-neutral wire protocol as a way for out-of-process programs to remotely instruct the behavior of web browsers.
>
> Provided is a set of interfaces to discover and manipulate DOM elements in web documents and to control the behavior of a user agent. It is primarily intended to allow web authors to write tests that automate a user agent from a separate controlling process, but may also be used in such a way as to allow in-browser scripts to control a — possibly separate — browser." - [w3.org/webdriver](https://www.w3.org/TR/webdriver/)

The underlying protocol that WebdriverIO implements for communicating to Appium.

<br/>

#### Selenium WebDriver

Also-known-as 'WebDriver' or 'Selenium 2' in the realm of Selenium. Not to be confused with 'selenium-webdriver' which is it's Node implementation <sup>[3](https://www.seleniumhq.org/projects/webdriver/)</sup>

> "If you want to: create robust, browser-based regression automation suites and tests; scale and distribute scripts across many environments
>
> Then you want to use Selenium WebDriver; a collection of language specific bindings to drive a browser -- the way it is meant to be driven." - [seleniumhq.org](https://www.seleniumhq.org/), [github.com/SeleniumHQ/selenium](https://github.com/SeleniumHQ/selenium/)

Although not used in this tutorial, but is Java implementation of (W3C) 'WebDriver' specification.

<br/>

#### selenium-webdriver (JS)

Also-known-as 'WebDriverJS'. Its Node module name is, 'selenium-webdriver'

> "WebDriverJs is the Official javascript implementation of selenium. It uses the Selenium Json-wire-protocol to interact with browser as selenium java does. It is written by selenium guys. Other tools like protractor depends on WebdriverJs to interact with browser. Webdriverjs is packaged as 'selenium-webdriver' under npm package which runs on nodejs." - [webdriverjs.com](http://www.webdriverjs.com), [github.com/SeleniumHQ/selenium/.../javascript/node/selenium-webdriver](https://github.com/SeleniumHQ/selenium/tree/master/javascript/node/selenium-webdriver), [seleniumhq.github.io/selenium/.../javascript](https://seleniumhq.github.io/selenium/docs/api/javascript/)

Although not used in this tutorial, but is a JavaScript implementation of 'Selenium WebDriver'.

<br/>

#### WebdriverIO

> "WebdriverIO is a test automation framework that allows you to run tests based on the Webdriver protocol and Appium automation technology. It provides support for your favorite BDD/TDD test framework and will run your tests locally or in the cloud using Sauce Labs, BrowserStack or TestingBot."
>
> "This binding is the most non-opinionated you will find as it just represents the WebDriver specification and doesn't come with any extra or higher level abstraction. It is lightweight and comes with support for the WebDriver specification and Appiums Mobile JSON Wire Protocol." - [github.com/webdriverio](https://github.com/webdriverio/webdriverio), [github.com/webdriverio/.../webdriver](https://github.com/webdriverio/webdriverio/tree/master/packages/webdriver)

This is what we will use for our e2e (end-to-end) code and will allow Appium to "drive" in automating our requests.

<br/>

#### Chromedriver

> "WebDriver is an open source tool for automated testing of web apps across many browsers. It provides capabilities for navigating to web pages, user input, JavaScript execution, and more.
>
> ChromeDriver is a standalone server which implements WebDriver's wire protocol for Chromium. We are in the process of implementing and moving to the W3C standard. ChromeDriver is available for Chrome on Android and Chrome on Desktop (Mac, Linux, Windows and ChromeOS)." - [chromedriver.chromium.org](http://chromedriver.chromium.org/)

Functions as a member in this triad of software for this tutorial: [Chrome](https://www.google.com/chrome/), [appium-chromedriver](https://github.com/appium/appium-chromedriver) and [chromedriver.exe](http://chromedriver.chromium.org/downloads).

<br/>

#### Protractor

> "Protractor is an end-to-end test framework for Angular and AngularJS applications. Protractor runs tests against your application running in a real browser, interacting with it as a user would."
>
> "Protractor is built on top of WebDriverJS, which uses native events and browser-specific drivers to interact with your application as a user would" - [protractortest.org](https://www.protractortest.org)

Although not used in this tutorial, its included when an Angular project is created. It interacts with WebDriverJS in someway to send requests to Selenium Server.

<br/>

### Unit Test Framework

#### Jasmine

> "Jasmine is a behavior-driven development framework for testing JavaScript code. It does not depend on any other JavaScript frameworks. It does not require a DOM. And it has a clean, obvious syntax so that you can easily write tests." - [jasmine.github.io](https://jasmine.github.io/)

The code that will be used in our e2e directory uses Jasmine for test suites and specs. Specifically the global Jasmine `describe` and `it` functions.

<br/>

### Locator Strategies

These strategies are used to interact with the WebView's DOM when interacting with hybrid and mobile web apps. When using these strategies, it can only work if the context of the app is set to WebView (via `WebViewScreen.switchToContext(CONTEXT_REF.WEBVIEW)` in this tutorial) and not Native. This switching of context can be seen in the `app.swipe.spec.ts` file. It's important to understand that the string value that is passed in `$()` determines what it returns. That may sound obvious for some, but not for your IDE, which when compiled may throw an exception.

#### CSS Query Selector

```javascript
const elem = $('h2.subheading a');
elem.click();
```

This is the only locator strategy we will use in this tutorial.

#### Link Text

```javascript
const link = $('=WebdriverIO');
console.log(link.getText());
```

#### Partial Link Text

```javascript
const link = $('*=driver');
console.log(link.getText());
```

#### Element with certain text

```javascript
const header = $('h1=Welcome to my Page');
console.log(header.getText());
```

#### Tag Name

```javascript
const classNameAndText = $('<my-element />');
console.log(classNameAndText.getText());
```

#### XPath

```javascript
const paragraph = $('//BODY/P[2]');
console.log(paragraph.getText())
```

Typically XPath strategy is avoided due to slow execution.

#### JS Function

```javascript
const elem = $('#elem');
elem.$(function () { return this.nextSibling.nextSibling })
```

<br/>

### Mobile Selectors Strategies

These are selector strategies are used to interact with native apps. In this tutorial we are automating a hybrid app, that is essentially a mobile web app as a subset to a native app being the superset.

When using these strategies, it can only work if the context of the app is set to Native (via `WebViewScreen.switchToContext(CONTEXT_REF.NATIVE)` in this tutorial) and not WebView. Although, this switching of context can be seen in the `app.swipe.spec.ts` file, the app in this tutorial is a hybrid which only needs to be switched for gestures.

#### UI Automator (Android, formerly known as 'UIAutomator 2.0')

 > "The UI Automator testing framework provides a set of APIs to build UI tests that perform interactions on user apps and system apps. The UI Automator APIs allows you to perform operations such as opening the Settings menu or the app launcher in a test device. The UI Automator testing framework is well-suited for writing black box-style automated tests, where the test code does not rely on internal implementation details of the target app." - [developer.android.com/.../ui-automator](https://developer.android.com/training/testing/ui-automator)

#### XCTest (iOS >= 10.0, 'UIAutomation' successor)

 > "Use the XCTest framework to write unit tests for your Xcode projects that integrate seamlessly with Xcode's testing workflow." - [developer.apple.com/.../xctest](https://developer.apple.com/documentation/xctest)

<br/>

## Target Audience

This article has been designed to target individuals that can assert any of the following statements about themselves:

* "I'm familiar with Ionic's toolchain for Android development and want to automate testing with Appium."
* "I'm familiar with Cordova's toolchain for Android development and want to automate testing with Appium."

As you might of concluded by now about the title being ambiguous, a more expanded (ridiculous) variation of this title can very well be, 'Android App Developed in Ionic 4 with Angular, Integrated with Cordova and Automated by Appium - driven by WebdriverIO'. Although specific, for those who will be using anything else, I'm sure you can gather some knowledge in what is typed below to fit some of your needs.

I will be using TypeScript, so make considerations if using JavaScript.

For those of you who are familiar with test automation as such, I'm going to be using synchronous execution mode versus asynchronous, as its less verbose and more importantly in test automation you typically want the process to be executed in a determined order. Also, just single capabilities will be used versus multiple.

Sorry for not including code for iOS as I don't have an Apple to do a build from. For those who want to test against iOS, take a look at [Wim Selles](https://github.com/wswebcreation)' [appium-boilerplate](https://github.com/webdriverio/appium-boilerplate) project on how iOS is set as a target for testing. I wouldn't think you need to diverge much from this tutorial app and his app. Afterall most of the `e2e/appium/` code was thankfully ported from his.

If it's in your interest, the following versions are installed on my Windows 10 machine:

```shell
>> node -v
>> nvm v
>> npm -v
>> ionic -v
v11.0.0
1.1.7
6.9.0
4.10.3
```

## Develop Our App To Be Tested

The animated gif below is 'Ionic4 Angular With Appium' (which is named 'myApp' in package.json file) as 'app under test' (AUT). This hopefully will illustrate to you the basic layout of the app. As can seen from the diagnostic information on top, Android's 'Pointer location' is enabled in the 'Developer options' of this device:

 ![AUT Recording](/assets/2019-03-21/App_Recording.gif#indent)

You have the option to clone (or fork) ['Ionic4 Angular With Appium'](https://github.com/marckassay/Ionic4AngularWithAppium) app that we will be used in this tutorial. For those who choose to proceed in development of this app, continue to step 1. Otherwise with your 'Ionic4 Angular With Appium' cloned, skip to 'Start AUT Process' section of this tutorial.

1. Execute your choice of Node package manager’s install command for `ionic` and `cordova` modules, if not already installed. For the sake of simplicity I will be using `npm` commands for this article. You may choose to use any other Node package manager, however with `cordova-fetch` it unconditionally uses `npm` which will conflict with `yarn` if used. For more information read about [spypkg's objective](https://github.com/marckassay/spypkg):

   ```shell
   $ /> npm install ionic cordova -g
   ```

2. By default, `ionic start` will add Angular dependencies and you may opt-out of appflow integration when prompt at the end of install. So execute the following to have ionic generate our app named 'myApp':

   ```shell
   $ /> ionic start myApp sidemenu
   ```

3. Change your CLI directory to 'myApp' and you should be able to at least build and have it served locally by executing what is below. For those who immediately receive an error, see the message below this block of code:

   ```shell
   $ /> cd myApp/
   $ /myApp> ionic serve
   ```

   *If you receive the following error message:*

   ```shell
   > ng serve
   'sh' is not recognized as an internal or external command,
   operable program or batch file.
   [ERROR] ng has unexpectedly closed (exit code 1).
   ```

   *A workaround is to change the name of `ng` file:*

   ```powershell
   $ /myApp> Rename-Item .\node_modules\.bin\ng -NewName ng.tmp
   ```

   *Or simply delete that file. This* [issue](https://github.com/ionic-team/ionic-cli/issues/3852) *is currently open in ionic-cli project.*

4. Generate a left and right sidemenu. This app will have a left and right sidemenu containing a list of items. I'm going to use some parts of the generated code from step 2 to quickly have us develop this app.

   ```shell
   $ /myApp> ionic generate page leftmenu
   $ /myApp> ionic generate page rightmenu
   ```

5. Now modify the following file in `src/app/`:

   ```code
   file: src/app/app.component.html
   ```

   ```html
   <ion-app>
     <ion-menu side="start">
         <app-leftmenu></app-leftmenu>
     </ion-menu>
     <ion-menu side="end">
         <app-rightmenu></app-rightmenu>
     </ion-menu>
     <ion-router-outlet main></ion-router-outlet>
   </ion-app>
   ```

   <br/>

   ```code
   file: src/app/app.component.ts
   ```

   ```typescript
   import { Component } from '@angular/core';
   import { Router } from '@angular/router';
   import { SplashScreen } from '@ionic-native/splash-screen/ngx';
   import { StatusBar } from '@ionic-native/status-bar/ngx';
   import { Platform } from '@ionic/angular';


   @Component({
     selector: 'app-root',
     templateUrl: 'app.component.html'
   })
   export class AppComponent {
     public appPages = [
       {
         title: 'Home',
         url: '/home',
         icon: 'home'
       },
       {
         title: 'List',
         url: '/list',
         icon: 'list'
       }
     ];

     constructor(
       private platform: Platform,
       private splashScreen: SplashScreen,
       private statusBar: StatusBar,
       private router: Router
     ) {
       this.initializeApp();
     }

     initializeApp() {
       this.platform.ready().then(() => {
         this.statusBar.styleDefault();
         this.splashScreen.hide();
         this.router.navigate(['/home']);
       });
     }
   }
   ```

   <br/>

   ```code
   file: src/app/app.module.ts
   ```

   ```typescript
   import { CUSTOM_ELEMENTS_SCHEMA, NgModule } from '@angular/core';
   import { BrowserModule } from '@angular/platform-browser';
   import { RouteReuseStrategy } from '@angular/router';
   import { SplashScreen } from '@ionic-native/splash-screen/ngx';
   import { StatusBar } from '@ionic-native/status-bar/ngx';
   import { IonicModule, IonicRouteStrategy } from '@ionic/angular';
   import { AppRoutingModule } from './app-routing.module';
   import { AppComponent } from './app.component';
   import { LeftmenuPageModule } from './leftmenu/leftmenu.module';
   import { RightmenuPageModule } from './rightmenu/rightmenu.module';

   @NgModule({
     declarations: [AppComponent],
     entryComponents: [],
     imports: [
       LeftmenuPageModule,
       RightmenuPageModule,
       BrowserModule,
       IonicModule.forRoot(
         {
           menuType: 'reveal'
         }),
       AppRoutingModule
     ],
     providers: [
       StatusBar,
       SplashScreen,
       { provide: RouteReuseStrategy, useClass: IonicRouteStrategy }
     ],
     bootstrap: [AppComponent],
     schemas: [CUSTOM_ELEMENTS_SCHEMA]
   })
   export class AppModule { }
   ```

6. Now for `LeftmenuPage` and `RightmenuPage`, modify their `@NgModule` tag that is located in their module file so that they can be imported:

   ```code
   file: src/app/leftmenu/leftmenu.module.ts
   ```

   ```typescript
   @NgModule({
     imports: [
       CommonModule,
       FormsModule,
       IonicModule,
       RouterModule.forChild(routes)
     ],
     declarations: [LeftmenuPage],
     exports: [LeftmenuPage]
   })
   ```

   <br/>

   ```code
   file: src/app/rightmenu/rightmenu.module.ts
   ```

   ```typescript
   @NgModule({
     imports: [
       CommonModule,
       FormsModule,
       IonicModule,
       RouterModule.forChild(routes)
     ],
     declarations: [RightmenuPage],
     exports: [RightmenuPage]
   })
   ```

   And replace all content between the curly braces of `LeftmenuPage` and `RightmenuPage` class with the following TypeScript code below.

   ```code
   file: src/app/leftmenu/leftmenu.page.ts
   file: src/app/rightmenu/rightmenu.page.ts
   ```

   ```typescript
   private selectedItem: any;
   private icons = [
     'flask',
     'wifi',
     'beer',
     'football',
     'basketball',
     'paper-plane',
     'american-football',
     'boat',
     'bluetooth',
     'build'
   ];
   public items: Array<{ title: string; note: string; icon: string }> = [];
   constructor() {
     for (let i = 1; i < 11; i++) {
       this.items.push({
         title: 'Item ' + i,
         note: 'This is item #' + i,
         icon: this.icons[ i - 1 ]
       });
     }
   }

   ngOnInit() {
   }
   ```

7. In an addition for `LeftmenuPage` and `RightmenuPage` modules, modify their html and scss file:

   Replace the `ion-content` tag with the following for:

   ```code
   file: src/app/leftmenu/leftmenu.page.html
   file: src/app/rightmenu/rightmenu.page.html
   ```

   ```html
   <ion-content>
     <ion-list>
       <ion-item *ngFor="let item of items">
         <ion-icon [name]="item.icon" slot="start"></ion-icon>
         {{item.title}}
         <div class="item-note" slot="end">
           {{item.note}}
         </div>
       </ion-item>
     </ion-list>
   </ion-content>
   ```

   Add the following scss code to files these files:

   ```code
   file: src/app/leftmenu/leftmenu.page.scss
   file: src/app/rightmenu/rightmenu.page.scss
   ```

   ```css
   :host {
   display: contents;
   }
   ```

8. Now that we are done with development. We need to add WebdriverIO dependencies. Execute the following to do just that.

   ```shell
   $ /myApp> npm install webdriverio @wdio/cli @wdio/jasmine-framework @wdio/local-runner @wdio/spec-reporter @wdio/sync -D
   ```

   If npm WARN ajv-keywords@3.4.0 requires a peer of ajv@^6.9.1 but none is installed. You must install peer dependencies yourself.

   ```shell
   $ /myApp> npm install ajv -D
   ```

9. Next, is to create the e2e tests. First restructure the e2e folder where Angular currently has it's karam tests. To separate Angular's karma e2e tests and our WDIO test, move all current contents of `e2e` folder into a new folder named `karma`. Create another folder in `e2e` named `appium`. So the `e2e` folder is now structured as illustrated:

   ![New e2e Structure](/assets/2019-03-21/New_e2e_Structure.png#indent)

10. Now we can add Appium and WDIO files. To do this simply [download the archive file](https://github.com/marckassay/Ionic4AngularWithAppium/raw/master/E2ECode.zip) that is the TypeScript port of [Wim Selles](https://github.com/wswebcreation)' JavaScript code from his [appium-boilerplate](https://github.com/webdriverio/appium-boilerplate) project. Also in this archive are typings I created. Once downloaded, extract contents to the project root `myApp` folder. So the `e2e/appium` folder is now structured as illustrated followed by typings folder:

    ![New e2e/appium Structure with Content](/assets/2019-03-21/New_e2e_Structure_With_Content.png#indent)

    ![Typings with Content](/assets/2019-03-21/Typings_Structure_With_Content.png#indent)

    I'll explain in the 'About the E2E Code' section of this tutorial about this code.

11. Edit `package.json` file to include `wdio:test` script command:

    ```json
    {
      "name": "myapp",
      "version": "0.0.1",
      "author": "Ionic Framework",
      "homepage": "https://ionicframework.com/",
      "private": true,
      "scripts": {
        "ng": "ng",
        "start": "ng serve",
        "build": "ng build",
        "test": "ng test",
        "lint": "ng lint",
        "e2e": "ng e2e",
        "wdio:test": "npx wdio ./e2e/appium/conf/wdio.conf.js"
      },
    ```

    Now this app's source and e2e code is completed.

12. This will integrate and build the app that will output the APK file:

    ```shell
    $ /myApp> ionic cordova build android
    ```

## About the E2E Code

```code
file: e2e/appium/constants.ts
```

Contains constants to be set per environment. All of these constants are applied in `wdio.conf.ts` file. You need to set the members in `constants.ts` according to your own environment! Read the comments in the `typings/` files, such as on how to determine an Android `Package` and `Activity` value.

<br/>

```code
file: e2e/conf/wdio.conf.js
file: e2e/conf/wdio.conf.ts
```

The `wdio.conf.js` file will call [ts-node](https://github.com/TypeStrong/ts-node) to execute our e2e TypeScript files so that Node (Node.js) can evaluate them.

The `wdio.conf.ts` file is our configuration. The exported member, `const config: WDIOConf` has been typed and commented to be read easily in an IDE.

<br/>

```code
file: e2e/appium/src/specs/app.swipe.spec.ts
```

This is our one-and-only test suite that contains 3 test specs. Notice how `WebViewScreen.switchToContext()` is being used whenever code needs to locate an element and make an assertion. It's also being used to switch to `CONTEXT_REF.NATIVE` when a gesture to be automated. That's important to know, otherwise it would fail.

<br/>

```code
file: typings/android-typings.d.ts
file: typings/appium-typings.d.ts
```

These TypeScript typings are used for `wdio.conf.ts` file. The `appium-typings.d.ts` file has general Appium capabilities and the `android-typings.d.ts` has Android specific capability members

## Start AUT Process

1. Add Appium dependencies. In this step the correct version of chromedriver will be installed.

    ```shell
    $ /myApp> npm install appium-doctor appium-uiautomator2-driver -D
    ```

2. You may install Appium as a global Node dependency, but I prefer using desktop application so that you can interact with the AUT when needed for standalone tests (which will not be discussed in this article). To install the desktop application, [download the installer](https://github.com/appium/appium-desktop/releases/tag/v1.11.0) for your platform and execute it. This installer, launches Appium server, which will install the 'Appium Settings' mobile app when putting our 'myApp' under-test for the first time.

3. Run Appium as admin.

   ![Run Appium as Admin](/assets/2019-03-21/Run_Appium_As_Admin.png#indent)

4. Now start Appium.

   ![Start Appium](/assets/2019-03-21/Start_Appium.png#indent)

   ![Appium Running](/assets/2019-03-21/Appium_Running.png#indent)

   More information on Appium Desktop can be found [here](https://github.com/appium/appium-desktop).

5. Finally, we are ready to run our test suite and have Appium "drive" our WebdriverIO tests. Execute the `wdio:test` script command:

   ```shell
   $ /myApp> npm run wdio:test
   ```

   Appium Settings app should of auto-installed and continue to run in the background. Hopefully after WDIO and Appium are finished, your CLI and Appium console should be similar to what is illustrated below:

    ![Post WDIO Test](/assets/2019-03-21/Post_WDIO_Test.png#indent)

   This concludes the tutorial for this article.

## Conclusion

Important takeaways for this article are:

* Understand what "context" is for an app. When reading documentation on e2e testing, determine how it applies to your app type (Native, Hybrid, Mobile). Because, depending on what you're attempting to do, Native recommendations may not apply to Hybrid and Mobile. See the 'Locator Strategies' and 'Mobile Selectors Strategies' for more information.

* Understand what configuration fields will need to be adjusted per project. For this tutorial, this is all set in the `constants.ts` file.

* Accept the state of software as it is today. Some frameworks you use are not funded or even have a full-time developer. And as a result, documentation may not reflect current state of source code. So expect these inconsistencies when using open-source that is free. This applies especially for, "software that tests software" that comes second to source code framework development in regards to resources.

I hope I explained this clearly or at the very least clarified a thing or two. This is my first article, any feedback is appreciated. Thanks!

I do plan to maintain this article and update it as needed. So if you find it can be improved submit a PR and I'll add your name to this article as a contributor.
