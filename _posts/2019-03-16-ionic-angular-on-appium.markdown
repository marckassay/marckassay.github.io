---
layout: post
title:  "Ionic 4 with Angular on Appium"
date:   2019-03-16 11:48:16 -0400
categories: test automation 
---

If you're familiar with test automation using Appium, the title of this article entails considerable amount of thought and knowledge on how an application is going to be tested successfully. Prior to typing this article, my tribulations of completing a successful test "spec" was such that I felt an article, hopefully this one, will remove some uncertainities for the next soul facing the same objectives.

This was my second experience with test automation, that both resulted similarly, not-so-pleasant. From the resources that I read online seem to conflict with what I was experinceing when implemented in my IDE. And not realizing the difference between the implementation framework and its specification added more confusion. At any rate, I can expend more energy stating nuisances but how is that going to help you reach your objective? So let me "paint" the landscape of siginificant frameworks, and protocals and strategies that will be used in this setup of "Ionic 4 with Angular on Appium".

### Prelude 

The following conceptual diagram will hopefully illustrate the landscape accurately that you might immerge yourself in here shortly:

    [insert diagram (https://github.com/d3/d3-sankey) of native selectors and "webdriver" selectors]
    [insert diagram https://observablehq.com/@mbostock/tree-of-life) of native selectors and "webdriver" selectors]
    [insert diagram https://observablehq.com/@d3/radial-dendrogram  ) of native selectors and "webdriver" selectors]

And before I start my narration about that diagram, I would like for you to read the following list that briefly describes about each "species" in this landscape:

  * Ionic - "... is the free, open source mobile UI toolkit for developing high-quality cross-platform apps for native iOS, Android, and the web—all from a single codebase." -https://ionicframework.com/

  * Angular - "Learn one way to build applications with Angular and reuse your code and abilities to build apps for any deployment target. For web, mobile web, native mobile and native desktop." - https://angular.io/

  * AngularJS (going extinct) - "... is a JavaScript-based open-source front-end web framework mainly maintained by Google and by a community of individuals and corporations to address many of the challenges encountered in developing single-page applications." - https://en.wikipedia.org/wiki/AngularJS
  
  * Appium - "Appium is an open source test automation framework for use with native, hybrid and mobile web apps.

    It drives iOS, Android, and Windows apps using the WebDriver protocol." - http://appium.io/

  * WebDriver (the W3C specification) - "WebDriver is a remote control interface that enables introspection and control of user agents. It provides a platform- and language-neutral wire protocol as a way for out-of-process programs to remotely instruct the behavior of web browsers.

    Provided is a set of interfaces to discover and manipulate DOM elements in web documents and to control the behavior of a user agent. It is primarily intended to allow web authors to write tests that automate a user agent from a separate controlling process, but may also be used in such a way as to allow in-browser scripts to control a — possibly separate — browser." - https://www.w3.org/TR/webdriver/

  * "Selenium WebDriver" (aka 'WebDriver' or 'Selenium 2'. in the realm of Selenium. Not to be confused with 'selenium-webdriver' which is it's node.js implementation)[ref](https://www.seleniumhq.org/projects/webdriver/) - "If you want to: create robust, browser-based regression automation suites and tests; scale and distribute scripts across many environments Then you want to use Selenium WebDriver; a collection of language specific bindings to drive a browser -- the way it is meant to be driven." - https://www.seleniumhq.org/ - https://github.com/SeleniumHQ/selenium/

  * selenium-webdriver (JS) (aka 'WebDriverJS'. nodejs module name: 'selenium-webdriver') - "WebDriverJs is the Official javascript implementation of selenium. It uses the Selenium's Json-wire-protocol to interact with browser as selenium java does. It is written by selenium guys. Other tools like protractor depends on WebdriverJs to interact with browser. Webdriverjs is packaged as 'selenium-webdriver' under npm package which runs on nodejs." - http://www.webdriverjs.com - https://github.com/SeleniumHQ/selenium/tree/master/javascript/node/selenium-webdriver - https://seleniumhq.github.io/selenium/docs/api/javascript/

  * selenium-webdriver (Ruby) (its ruby gem name is: 'selenium-webdriver')- "WebDriver is a tool for writing automated tests of websites. It aims to mimic the behaviour of a real user, and as such interacts with the HTML of the application." - rubygems.org/gems/selenium-webdriver

  * WebdriverIO - "WebdriverIO is a test automation framework that allows you to run tests based on the Webdriver protocol and Appium automation technology. It provides support for your favorite BDD/TDD test framework and will run your tests locally or in the cloud using Sauce Labs, BrowserStack or TestingBot." + "This binding is the most non-opinionated you will find as it just represents the WebDriver specification and doesn't come with any extra or higher level abstraction. It is lightweight and comes with support for the WebDriver specification and Appiums Mobile JSONWire Protocol." - https://github.com/webdriverio/webdriverio + https://github.com/webdriverio/webdriverio/tree/master/packages/webdriver

  * Chromedriver - "WebDriver is an open source tool for automated testing of webapps across many browsers. It provides capabilities for navigating to web pages, user input, JavaScript execution, and more. ChromeDriver is a standalone server which implements WebDriver's wire protocol for Chromium. We are in the process of implementing and moving to the W3C standard. ChromeDriver is available for Chrome on Android and Chrome on Desktop (Mac, Linux, Windows and ChromeOS)." - http://chromedriver.chromium.org/

  * Protractor - "Protractor is an end-to-end test framework for Angular and AngularJS applications. Protractor runs tests against your application running in a real browser, interacting with it as a user would.", "Protractor is built on top of WebDriverJS, which uses native events and browser-specific drivers to interact with your application as a user would" - https://www.protractortest.org

  * Jasmine - "Jasmine is a behavior-driven development framework for testing JavaScript code. It does not depend on any other JavaScript frameworks. It does not require a DOM. And it has a clean, obvious syntax so that you can easily write tests." - https://jasmine.github.io/

  * Mocha - "Mocha is a feature-rich JavaScript test framework running on Node.js and in the browser, making asynchronous testing simple and fun. Mocha tests run serially, allowing for flexible and accurate reporting, while mapping uncaught exceptions to the correct test cases." - https://mochajs.org/

  * Karma - "Karma is essentially a tool which spawns a web server that executes source code against test code for each of the browsers connected. The results of each test against each browser are examined and displayed via the command line to the developer such that they can see which browsers and tests passed or failed." - https://karma-runner.github.io/3.0/intro/how-it-works.html

  * Chai - "Chai is a BDD / TDD assertion library for node and the browser that can be delightfully paired with any javascript testing framework." - https://www.chaijs.com/

  * Cuccumber - https://cucumber.io/

  * Selenium - "Selenium automates browsers. That's it! What you do with that power is entirely up to you. Primarily, it is for automating web applications for testing purposes, but is certainly not limited to just that. Boring web-based administration tasks can (and should!) be automated as well.

    Selenium has the support of some of the largest browser vendors who have taken (or are taking) steps to make Selenium a native part of their browser. It is also the core technology in countless other browser automation tools, APIs and frameworks." - https://www.seleniumhq.org/

  * JSON Wire Protocol - "... a client-server protocol (known as the JSON Wire Protocol)"

  * Mobile JSON Wire Protocol Specification - "Appium implements the Mobile JSONWP [aka MJSONWP] which is the extension of the Selenium JSONWP. Mobile JSONWP controls the behaviors of different mobile devices such as installing and uninstalling of apps over the session.

    The need for automation of native and hybrid mobile applications can be met by the extension of the JSONWP, which already has a proven basic automation framework (architecture, interaction model, etc...)." - https://www.programsbuzz.com/article/mobile-json-wire-protocol

  * Locator/Selector Strategies

        CSS Query Selector

        Link Text

        Partial Link Text

        Element with certain text

        Tag Name

        XPath

        JS Function

  * Webview (Android) - "In most cases, we recommend using a standard web browser, like Chrome, to deliver content to the user. To learn more about web browsers, read the guide on invoking a browser with an intent.

    WebView objects allow you to display web content as part of your activity layout, but lack some of the features of fully-developed browsers. A WebView is useful when you need increased control over the UI and advanced configuration options that will allow you to embed web pages in a specially-designed environment for your app." - <https://developer.android.com/reference/android/webkit/WebView>

  * WKWebView (iOS) - "An object that displays interactive web content, such as for an in-app browser.", "Starting in iOS 8.0 and OS X 10.10, use WKWebView to add web content to your app. Do not use UIWebView or WebView." - <https://developer.apple.com/documentation/webkit/wkwebview>

  * Mobile Selectors

        UI Automator (Android, formally know as 'UIAutomator 2.0') - "The UI Automator testing framework provides a set of APIs to build UI tests that perform interactions on user apps and system apps. The UI Automator APIs allows you to perform operations such as opening the Settings menu or the app launcher in a test device. The UI Automator testing framework is well-suited for writing black box-style automated tests, where the test code does not rely on internal implementation details of the target app." - https://developer.android.com/training/testing/ui-automator

        UIAutomation (iOS < 10.0) - *see XCTest*

        XCTest (iOS >= 10.0) - "Use the XCTest framework to write unit tests for your Xcode projects that integrate seamlessly with Xcode's testing workflow." - https://developer.apple.com/documentation/xctest

        UiAutomator2 (Appium) - "Appium's flagship support for automating Android apps is via the UiAutomator2 driver. This driver leverages Google's UiAutomator2 [known now as 'UI Automator'] technology to facilitate automation on a device or emulator." - http://appium.io/docs/en/drivers/android-uiautomator2/

  * Native apps - Hybrid apps - Mobile web apps  - CONTEXT

        [insert diagram (https://github.com/d3/d3-sankey) of 3 app types]

### Other

"...allows you to write code as if WebDriverJS had a synchronous, blocking API (like all of the other Selenium language bindings)." - https://github.com/SeleniumHQ/selenium/wiki/WebDriverJs

"One common feature of mobile platforms is the ability to embed a chromeless webbrowser inside of a 'native' application. These are called 'webviews', and, if possible, a server for a given platform should implement support for automating the webview using the full, regular, WebDriver API." - https://github.com/SeleniumHQ/mobile-spec/blob/master/spec-draft.md#webviews-and-other-contexts

"The need for automation of native and hybrid mobile applications can be met by the extension of the JSONWP" - https://github.com/SeleniumHQ/mobile-spec/blob/master/spec-draft.md#webviews-and-other-contexts + "Appium client libraries implement the Mobile JSON Wire Protocol" - http://appium.io/docs/en/about-appium/appium-clients/index.html

"These libraries wrap standard Selenium client libraries to provide all the regular selenium commands dictated by the JSON Wire protocol, and add extra commands related to controlling mobile devices, such as multi-touch gestures and screen orientation. 

    Appium client libraries implement the Mobile JSON Wire Protocol" - http://appium.io/docs/en/about-appium/appium-clients/index.html

      AppiumLibCore	https://github.com/appium/ruby_lib
      # https://github.com/appium/ruby_lib_core
      Appium Python Client	https://github.com/appium/python-client
      java-client	https://github.com/appium/java-client
      WD.js https://github.com/admc/wd
      WebdriverIO	https://github.com/webdriverio/webdriverio
      web2driver https://github.com/projectxyzio/web2driver
      Selenium.framework https://github.com/appium/selenium-objective-c
      Appium PHP Client	https://github.com/appium/php-client
      appium-dotnet-driver	https://github.com/appium/appium-dotnet-driver
      RobotFramework	https://github.com/jollychang/robotframework-appiumlibrary

"[Selenium] WebDriver is the name of the key interface against which tests should be written in Java, the implementing classes one should use are listed as below:

    ChromeDriver, EventFiringWebDriver, FirefoxDriver, HtmlUnitDriver, InternetExplorerDriver, PhantomJSDriver, RemoteWebDriver, SafariDriver" - https://www.seleniumhq.org/projects/webdriver/

"ChromeDriver is a standalone server which implements WebDriver's wire protocol for Chromium. 
    We are in the process of implementing and moving to the W3C standard. ChromeDriver is available for Chrome on Android and Chrome on Desktop (Mac, Linux, Windows and ChromeOS).  

    You can view the current implementation status of the WebDriver standard here." - http://chromedriver.chromium.org/

"language bindings or clients"
 Webdriverio -> Appium -> UIAutomater2

Example of Selenium ChromeDriver ports of different languages:
    https://seleniumhq.github.io/selenium/docs/api/dotnet/html/T_OpenQA_Selenium_Chrome_ChromeDriver.htm
    https://seleniumhq.github.io/selenium/docs/api/rb/Selenium/WebDriver/Chrome/Driver.html
    https://seleniumhq.github.io/selenium/docs/api/java/index.html
    https://seleniumhq.github.io/selenium/docs/api/javascript/module/selenium-webdriver/chrome_exports_Driver.html
    https://seleniumhq.github.io/selenium/docs/api/py/webdriver_chrome/selenium.webdriver.chrome.webdriver.html#module-selenium.webdriver.chrome.webdriver


### Selenium Client & WebDriver Language Bindings

"In order to create scripts that interact with the Selenium Server (Selenium RC, Selenium Remote WebDriver) or create local Selenium WebDriver scripts, you need to make use of language-specific client drivers. These languages include both 1.x and 2.x style clients.

While language bindings for other languages exist, these are the core ones that are supported by the main project hosted on GitHub." [ref](https://www.seleniumhq.org/download/)

  Java	3.141.59	2018-11-14	Download  	Change log  	Javadoc
  C#	3.14.0	2018-08-02	Download	Change log	API docs
  Ruby	3.14.0	2018-08-03	Download	Change log	API docs
  Python	3.14.0	2018-08-02	Download	Change log	API docs
  Javascript (Node)	4.0.0-alpha.1	2018-01-13	Download	Change log	API docs

#### Third Party Language Bindings NOT DEVELOPED by seleniumhq
    Perl download and docs by Gordon Child
    Perl 6 by Ahmad M. Zawawi
    PHP by Chibimagic (real name unknown?)
    PHP by Lukasz Kolczynski
    PHP by facebook
    PHP by Adam Goucher
    PHP by Nearsoft
    Haskell by Adam Curtis
    Objective-C by Dan Cuellar
    Javascript by Adam Christian
    Javascript by Jonathan Lipps
    Javascript by Camilo Tapia, Vincent Voyer and Christian Bromann
    JavaScript Leadfoot by SitePen
    R by John Harrison
    Dart by Marc Fisher
    Tcl by Tobias Koch
    Elixir by Nathan Johnson

https://github.com/christian-bromann/awesome-selenium#drive

### Prerequisites

As I mentioned the title entails considerable amount of knowledge, a more specific (ridiculous) variation can very well be "Android Application Developed in Ionic 4 with Angular, Intergrated with Cordova and Automated by Appium - driven by WebdriverIO". Although specific and long, for those who will be using anything else, I'm sure you can gather some knowledge in what is typed below to fit some of your needs.

### Develop The Application To Be Under Test

You will need to create an Ionic 4 application with Angular. Execute your choice of Nodejs package manager install command (for the sake of simplicity I will be using `npm` commands for this article) if not already installed for `ionic` and `cordova` modules:

  1. if you haven't installed ionic or cordova globally:

    ```npm install ionic cordova -g```

  2. by default angular is installed. and you opt-out of appflow intergration when prompt at the end of install:

    ```ionic start myApp sidemenu```

  3. hopefully you should be able to at least build and have it served locally:

    ```ionic serve```

  4. generate a left and right sidemenu:

    ```ionic generate page leftmenu```
    ```ionic generate page rightmenu```

    TIP: if you receieve the following error message:
    ```shell
        > ng generate page leftmenu
        'sh' is not recognized as an internal or external command,
        operable program or batch file.
        [ERROR] Could not generate page.
    ```

    A workaround is to change the name of ```ng``` file:
    	E:\temp\myApp [master]> rename-item .\node_modules\.bin\ng -NewName ng.tmp

  5. now modify ```AppModule``` so that its files are to the following:

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
<br>

 ```src\app\app.component.ts```
 ```typescript
  initializeApp() {
      this.platform.ready().then(() => {
          this.statusBar.styleDefault();
      this.splashScreen.hide();
      this.router.navigate(['/home']);
    });
  }
 ```
<br>

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

  6. now for ```LeftmenuPage``` and ```RightmenuPage```, modify the their ```@NgModule``` tag that is located in their module file so that they are exportable:

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
<br>

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

 7. in an addition for ```LeftmenuPage``` and ```RightmenuPage```, add the following to their scss file:

 ```src\app\leftmenu\leftmenu.page.scss``` and
 ```src\app\rightmenu\rightmenu.page.scss```
```sass
 :host {
  display: contents;
}
```

8. add WebdriverIO dependencies 

```shell
npm install webdriverio -D
npm install webdriverio @wdio/cli @wdio/jasmine-framework @wdio/local-runner @wdio/spec-reporter @wdio/sync -D
```

npm WARN ajv-keywords@3.4.0 requires a peer of ajv@^6.9.1 but none is installed. You must install peer dependencies yourself.

if so 

```shell
npm install ajv -D
```

  9. add Appium dependencies

```shell
npm install appium-doctor appium-uiautomator2-driver -D
```

  10.

  edit package.json file to include new script command:
```
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
### The Application built Cordova

This will intergate and build the application

```shell
ionic cordova build android --debug
```

./platforms/android/app/build/outputs/apk/debug/app-debug.apk

### Deploy Application

```
adb install .\platforms\android\app\build\outputs\apk\debug\app-debug.apk
```

### Install Appium 

You may, install the global dependency of appium, but I recommend the desktop app so that you can visually see the AUT.

To install the desktop, which is the server download the installer for your platform here:
https://github.com/appium/appium-desktop/releases/tag/v1.11.0

And this will install the Appium Settings App when launched

### Start Appium Server

launch Appium as admin

### Execute WebdriverIO

```shell
npm run wdio:test
```