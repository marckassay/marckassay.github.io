---
layout: post
title:  "Ionic 4 Angular with Appium"
date:   2019-03-21 11:48:16 -0400
categories: test automation 
---

If you're familiar with test automation using Appium, the title of this article entails considerable amount of thought and knowledge on how an application is going to be tested successfully. Prior to typing this article, my tribulations of completing a successful test "spec" was such that I felt an article, hopefully this one, will remove some uncertainities for the next soul facing the same objectives.

This was my second experience with this sort of test automation, that both required tremendous amount of research and trial-and-errors. From the documentation that I read online seem to freqeuntly conflict with what I experinced when implemented in my IDE. Having implmentation frameworks being referred to the same name as the specfication only added to the confusion when trying to construct, mentally, this test automation landscape. This landscape is vast with several layers of abstraction between frameworks. That said, and until perhaps a software analyst that specizlies in this realm explains in detail, my objective for this article is to simplely guide you to pass just a couple of test specs. Those test specs invovle, swipe gestures and assert that exepectd data is indeed shown. So let me explain (by clarifying), to the best of my knowledge, the landscape of siginificant frameworks, and protocals and strategies that will be used in this setup of "Ionic 4 Angular with Appium".

## Prelude

The following conceptual diagram will hopefully illustrate the landscape accurately that you might have to immerge yourself in shortly:

    [insert diagram (https://github.com/d3/d3-sankey) of native selectors and "webdriver" selectors]
    [insert diagram https://observablehq.com/@mbostock/tree-of-life) of native selectors and "webdriver" selectors]
    [insert diagram https://observablehq.com/@d3/radial-dendrogram  ) of native selectors and "webdriver" selectors]

Before we start with the tutorial section, read the brief descriptions below about framweworks and protocols that releated to this article:

* Ionic

  > "... is the free, open source mobile UI toolkit for developing high-quality cross-platform apps for native iOS, Android, and the web—all from a single codebase."
  
  [ionicframework.com](https://ionicframework.com/)

* Angular

  > "Learn one way to build applications with Angular and reuse your code and abilities to build apps for any deployment target. For web, mobile web, native mobile and native desktop."
  
  [angular.io](https://angular.io/)

* Appium

  > "Appium is an open source test automation framework for use with native, hybrid and mobile web apps.
  >
  > It drives iOS, Android, and Windows apps using the WebDriver protocol."

  [appium.io](http://appium.io/)

* WebDriver (the W3C specification)

  > "WebDriver is a remote control interface that enables introspection and control of user agents. It provides a platform- and language-neutral wire protocol as a way for out-of-process programs to remotely instruct the behavior of web browsers.
  >
  > Provided is a set of interfaces to discover and manipulate DOM elements in web documents and to control the behavior of a user agent. It is primarily intended to allow web authors to write tests that automate a user agent from a separate controlling process, but may also be used in such a way as to allow in-browser scripts to control a — possibly separate — browser."

  [w3.org/webdriver](https://www.w3.org/TR/webdriver/)

* Selenium WebDriver (aka 'WebDriver' or 'Selenium 2' in the realm of Selenium. Not to be confused with 'selenium-webdriver' which is it's node.js implementation <sup>[ref](https://www.seleniumhq.org/projects/webdriver/)</sup> )

  > "If you want to: create robust, browser-based regression automation suites and tests; scale and distribute scripts across many environments
  >
  > 0Then you want to use Selenium WebDriver; a collection of language specific bindings to drive a browser -- the way it is meant to be driven."

  [seleniumhq.org](https://www.seleniumhq.org/)

  [github.com/SeleniumHQ/selenium](https://github.com/SeleniumHQ/selenium/)

* selenium-webdriver (JS) (aka 'WebDriverJS'. nodejs module name: 'selenium-webdriver')

  > "WebDriverJs is the Official javascript implementation of selenium. It uses the Selenium's Json-wire-protocol to interact with browser as selenium java does. It is written by selenium guys. Other tools like protractor depends on WebdriverJs to interact with browser. Webdriverjs is packaged as 'selenium-webdriver' under npm package which runs on nodejs."
  
  [webdriverjs.com](http://www.webdriverjs.com)

  [github.com/SeleniumHQ/selenium/.../javascript/node/selenium-webdriver](https://github.com/SeleniumHQ/selenium/tree/master/javascript/node/selenium-webdriver)
  
  [seleniumhq.github.io/selenium/.../javascript](https://seleniumhq.github.io/selenium/docs/api/javascript/)

* WebdriverIO
  > "WebdriverIO is a test automation framework that allows you to run tests based on the Webdriver protocol and Appium automation technology. It provides support for your favorite BDD/TDD test framework and will run your tests locally or in the cloud using Sauce Labs, BrowserStack or TestingBot."
  >
  > "This binding is the most non-opinionated you will find as it just represents the WebDriver specification and doesn't come with any extra or higher level abstraction. It is lightweight and comes with support for the WebDriver specification and Appiums Mobile JSONWire Protocol."

  [github.com/webdriverio](https://github.com/webdriverio/webdriverio)
  
  [github.com/webdriverio/.../webdriver](https://github.com/webdriverio/webdriverio/tree/master/packages/webdriver)

* Chromedriver

  > "WebDriver is an open source tool for automated testing of webapps across many browsers. It provides capabilities for navigating to web pages, user input, JavaScript execution, and more.
  >
  > ChromeDriver is a standalone server which implements WebDriver's wire protocol for Chromium. We are in the process of implementing and moving to the W3C standard. ChromeDriver is available for Chrome on Android and Chrome on Desktop (Mac, Linux, Windows and ChromeOS)."

  [chromedriver.chromium.org](http://chromedriver.chromium.org/)

* Protractor

  > "Protractor is an end-to-end test framework for Angular and AngularJS applications. Protractor runs tests against your application running in a real browser, interacting with it as a user would."
  >
  > "Protractor is built on top of WebDriverJS, which uses native events and browser-specific drivers to interact with your application as a user would"

  [protractortest.org](https://www.protractortest.org)

* Jasmine

  > "Jasmine is a behavior-driven development framework for testing JavaScript code. It does not depend on any other JavaScript frameworks. It does not require a DOM. And it has a clean, obvious syntax so that you can easily write tests."

    [jasmine.github.io](https://jasmine.github.io/)

* Karma

  > "Karma is essentially a tool which spawns a web server that executes source code against test code for each of the browsers connected. The results of each test against each browser are examined and displayed via the command line to the developer such that they can see which browsers and tests passed or failed."

  [karma-runner.github.io/.../how-it-works.html](https://karma-runner.github.io/3.0/intro/how-it-works.html)

* Selenium

  > "Selenium automates browsers. That's it! What you do with that power is entirely up to you. Primarily, it is for automating web applications for testing purposes, but is certainly not limited to just that. Boring web-based administration tasks can (and should!) be automated as well.
  >
  >    Selenium has the support of some of the largest browser vendors who have taken (or are taking) steps to make Selenium a native part of their browser. It is also the core technology in countless other browser automation tools, APIs and frameworks."

  [seleniumhq.org](https://www.seleniumhq.org/)

* JSON Wire Protocol

  > "... a client-server protocol (known as the JSON Wire Protocol)"

* Mobile JSON Wire Protocol Specification

  > "Appium implements the Mobile JSONWP [aka MJSONWP] which is the extension of the Selenium JSONWP. Mobile JSONWP controls the behaviors of different mobile devices such as installing and uninstalling of apps over the session.
  >
  > The need for automation of native and hybrid mobile applications can be met by the extension of the JSONWP, which already has a proven basic automation framework (architecture, interaction model, etc...)."

  [programsbuzz.com/.../mobile-json-wire-protocol](https://www.programsbuzz.com/article/mobile-json-wire-protocol)

* Locator/Selector Strategies

  * CSS Query Selector
  * Link Text
  * Partial Link Text
  * Element with certain text
  * Tag Name
  * XPath
  * JS Function

* Webview (Android)

  > "In most cases, we recommend using a standard web browser, like Chrome, to deliver content to the user. To learn more about web browsers, read the guide on invoking a browser with an intent.
  >
  >  WebView objects allow you to display web content as part of your activity layout, but lack some of the features of fully-developed browsers. A WebView is useful when you need increased control over the UI and advanced configuration options that will allow you to embed web pages in a specially-designed environment for your app."

    [developer.android.com/.../webkit/WebView](https://developer.android.com/reference/android/webkit/WebView)

* WKWebView (iOS)

  > "An object that displays interactive web content, such as for an in-app browser."
  >
  > "Starting in iOS 8.0 and OS X 10.10, use WKWebView to add web content to your app. Do not use UIWebView or WebView."

   [developer.apple.com/.../webkit/wkwebview](https://developer.apple.com/documentation/webkit/wkwebview)

* Mobile Selectors

  * UI Automator (Android, formally know as 'UIAutomator 2.0')
  
    > "The UI Automator testing framework provides a set of APIs to build UI tests that perform interactions on user apps and system apps. The UI Automator APIs allows you to perform operations such as opening the Settings menu or the app launcher in a test device. The UI Automator testing framework is well-suited for writing black box-style automated tests, where the test code does not rely on internal implementation details of the target app."

    [developer.android.com/.../ui-automator](https://developer.android.com/training/testing/ui-automator)

  * UIAutomation (iOS < 10.0)

      *see XCTest section*

  * XCTest (iOS >= 10.0)
  
    > "Use the XCTest framework to write unit tests for your Xcode projects that integrate seamlessly with Xcode's testing workflow."

    [developer.apple.com/.../xctest](https://developer.apple.com/documentation/xctest)

  * UiAutomator2 (Appium)

    > "Appium's flagship support for automating Android apps is via the UiAutomator2 driver. This driver leverages Google's UiAutomator2 [now known just as 'UI Automator'] technology to facilitate automation on a device or emulator."

    [appium.io/.../android-uiautomator2](http://appium.io/docs/en/drivers/android-uiautomator2/)

  * Native apps - Hybrid apps - Mobile web apps  - CONTEXT

        [insert diagram (https://github.com/d3/d3-sankey) of 3 app types]

## Prerequisites

As I mentioned the title entails considerable amount of knowledge, a more specific (ridiculous) variation can very well be "Android Application Developed in Ionic 4 with Angular, Intergrated with Cordova and Automated by Appium - driven by WebdriverIO". Although specific and long, for those who will be using anything else, I'm sure you can gather some knowledge in what is typed below to fit some of your needs. Also I will be using TypeScript, so make considerations if using JavaScript.

If its in your interest, the following versions I'll be using are:

```shell
>> node -v
>> nvm v
>> npm -v
>> ionic -v
>> cordova -v
v11.0.0
1.1.7
6.9.0
4.10.3
8.1.2 (cordova-lib@8.1.1)
```

### Install Appium

You may, install appium as a global node dependency, but I prefer the desktop app so that you can interact with the AUT when needed for standalone tests.

To install the desktop, [download the installer](https://github.com/appium/appium-desktop/releases/tag/v1.11.0) for your platform and execute it. This installer, launches Appium server, which will install the 'Appium Settings' app when putitng our 'myApp' under-test for the first time. More on that in steps 12-16

## Develop The Application To Be Under Test

You have the opition to clone or fork the [repository](https://github.com/marckassay/Ionic4AngularWithAppium) for this application here.

You will need to create an Ionic 4 application with Angular. Execute your choice of Nodejs package manager install command (for the sake of simplicity I will be using `npm` commands for this article) if not already installed for `ionic` and `cordova` modules:

1. If you haven't installed ionic or cordova globally:

    ```shell
    npm install ionic cordova -g
    ```

2. By default angular is installed. You may opt-out of appflow intergration when prompt at the end of install:

    ```shell
    ionic start myApp sidemenu
    ```

3. Hopefully you should be able to at least build and have it served locally:

    ```shell
    ionic serve
    ```

4. Generate a left and right sidemenu:

    ```shell
    ionic generate page leftmenu
    ionic generate page rightmenu
    ```

    *If you receieve the following error message:*

    ```shell
    > ng generate page leftmenu
    'sh' is not recognized as an internal or external command,
    operable program or batch file.
    [ERROR] Could not generate page.
    ```

    *A workaround is to change the name of ```ng``` file:*

    ```shell
    E:\myApp> Rename-Item .\node_modules\.bin\ng -NewName ng.tmp
    ```


    *Or simplely remove it. This [issue](https://github.com/yarnpkg/yarn/issues/6902) was reported from previous yarn installs on Windows, which is resolved in yarn 1.15.2. This also seems to occur with npm. If this does happen, make a mental note in case if your package manager overwrites this workaround.*

5. Now modify ```AppModule``` so that its files are to the following:

    ```src\app\app.component.html```

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

    ```src\app\app.component.ts```

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

    ```src\app\app.module.ts```

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

6. Now for ```LeftmenuPage``` and ```RightmenuPage```, modify the their ```@NgModule``` tag that is located in their module file so that they are exportable:

    ```src\app\leftmenu\leftmenu.module.ts```

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

    ```src\app\rightmenu\rightmenu.module.ts```

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

    Replace all content between the curly braces of `LeftmenuPage` and `RightmenuPage` class with TypeScript below.

    ```src\app\leftmenu\leftmenu.page.ts``` and ```src\app\rightmenu\rightmenu.page.ts```
    
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
          icon: this.icons[Math.floor(Math.random() * this.icons.length)]
        });
      }
    }

    ngOnInit() {
    }
    ```

7. In an addition for ```LeftmenuPage``` and ```RightmenuPage``` modules, modify their html and scss file:

    Replace the these html files `ion-content` with the following html
    ```src\app\leftmenu\leftmenu.page.html``` and ```src\app\rightmenu\rightmenu.page.html```

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

    Add the following scss files these files:
    ```src\app\leftmenu\leftmenu.page.html``` and ```src\app\rightmenu\rightmenu.page.html```

    ```src\app\leftmenu\leftmenu.page.scss``` and ```src\app\rightmenu\rightmenu.page.scss```

    ```sass
    :host {
    display: contents;
    }
    ```

8. Add WebdriverIO dependencies

    ```shell
    npm install webdriverio @wdio/cli @wdio/jasmine-framework @wdio/local-runner @wdio/spec-reporter @wdio/sync -D
    ```

    If npm WARN ajv-keywords@3.4.0 requires a peer of ajv@^6.9.1 but none is installed. You must install peer dependencies yourself.

    ```shell
    npm install ajv -D
    ```

9. Restructure e2e folder

    To seperate Angular's karma e2e tests and our WDIO test, move all current contents of ```e2e``` folder into a new folder named ```karma```. Create another folder in ```e2e``` named ```appium```. So the e2e folder is arranged as what illustrated:

    ![New e2e Structure](2019-03-21/New_e2e_Structure.png)

10. Add Appium and WDIO files

    To add the Appium and WDIO files, simplely [download the archive file](https://github.com/marckassay/Ionic4AngularWithAppium/e2e/appium/src/helpers) that is the TypeScript port of [Wim Selles](https://github.com/wswebcreation)' JavaScript files from his [appium-boilerplate](https://github.com/webdriverio/appium-boilerplate) project.

    Once downloaded, extract contents to the `e2e/appium/` folder that you created in step 9.

    ![New e2e Structure](2019-03-21/New_e2e_Structure_With_Content.png)

x11. Add Appium dependencies

    ```shell
    npm install appium-doctor appium-uiautomator2-driver -D
    ```

12. Edit package.json file to include new script command:

    ```json
    {
      "name": "myapp",
      "version": "0.0.1",
      "author": "Ionic Framework",
      "homepage": "https://ionicframework.com/",
      "scripts": {
        "ng": "ng",
        "start": "ng serve",
        "build": "ng build",
        "test": "ng test",
        "lint": "ng lint",
        "e2e": "ng e2e",
        "wdio:test": "npx wdio ./e2e/appium/config/wdio.config.js"
      },
      ...
    ```

### Intergate Cordova with the Application for Deployment

13. This will intergate and build the application:

    ```shell
    ionic cordova build android --debug
    ```

14. To deploy (and install) on a physical device, execute the following:

    ```shell
    adb install .\platforms\android\app\build\outputs\apk\debug\app-debug.apk
    ```

### Start Appium Server

15. Run Appium as admin.

  ![Run Appium as Admin](2019-03-21/Run_Appium_As_Admin.png)

16. Now start Appium.

  ![Start Appium](2019-03-21/Start_Appium.png)

  ![Appium Running](2019-03-21/Appium_Running.png)

### Execute WebdriverIO

17. Finally, we are ready to run our test suite. Execute this.

  ```shell
  npm run wdio:test
  ```

  Hopefully after WDIO and Appium are finished, your CLI and Appium console should be similar to what is illustrated below:

  ![Post WDIO test](2019-03-21/Post_WDIO_test.png)
