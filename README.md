## Ionic 2 Demo

A  clone of [this repo](https://github.com/balakirevs/clicker), which was in turn a clone of [this repo](https://github.com/lathonez/clicker), modified to demonstrate testing an Ionic 2 hybrid app with Protractor and Appium.

For now, only Android testing is covered.

Changes I've made so far to previous repo:

- Updated device info in *protractor-android.conf.js*
- Updated code in *protractor.conf.js* and *protractor-android.conf.js* to set  *SpecReporter*
- Added hack in *protractor-android.conf.js* to make `browser.get()` a no-op, to avoid opening a browser window when running on Android. See [this issue](https://github.com/lathonez/clicker/issues/122).

This allows some tests to run, but others fail with errors like this

```
1) ClickerApp the left menu has a link with title Clickers
  - Failed: unknown error: Element is not clickable at point (32, 28). Other element would receive the click: <div class="toolbar-title toolbar-title-md" ng-reflect-klass="toolbar-title" ng-reflect-ng-class="toolbar-title-md">...</div>
```

## Setup

Update *Android Studio* and the emulator.

Create at least one virtual device.

Install Appium, Appium Doctor, and ChromeDriver, as necessary.

```bash
npm install appium@latest -g 
npm install appium-doctor -g
```

With Appium 1.6.4 testing an emulation of a Nexus with Android 7.1.1, I get the error
`Chrome version must be >= 55.0.2883.0`. To fix, I downloaded the latest
ChromeDriver from [the download site](https://sites.google.com/a/chromium.org/chromedriver/downloads)
into */usr/local/bin* and pass the file path to Appium. See below.

Install and test Clicker.

```bash
git clone https://github.com/eecs394-s17/clicker.git
cd clicker
npm install
npm start                     # start the desktop application (ionic serve)
ionic serve -l                # OR check the app as rendered in multiple platforms
```

When done, exit the server.

Now try the unit tests. A web page with test results should appear. All tests should pass.

```bash
npm test
```

When working, exit the unit tester.

Now try the end-to-end tests.

```bash
npm run e2e
```

The web version of Clicker should appear, and automatically run through the tests
under the e2e directory. All tests should pass.

Now, add Android emulation.

```bash
ionic platform add android
ionic emulate android
```

The emulator should appear with your app. Verify that it works.

Edit the *capabilities* object in [protractor-android.conf.js](protractor-android.conf.js) to
have the correct device name, platform version, AVD name, and full path
to your Android `android-debug.apk` file. Shell shortcuts such as `~/` will not
work here.

```bash
  capabilities: {
    browserName: '',
    'appium-version': '1.6.4',
    platformName: 'android',
    platformVersion: '7.1.1',
    deviceName: 'emulator-5554',
    autoWebview: true,
    avd: 'Nexus_5X_API_25',
    nativeInstrumentsLib: true,
    app: "/full/path/to/your/apk/android-debug.apk"
  },
```

Use Appium Doctor to check your settings

```bash
appium-doctor --android
```

Start the Appium server.

```bash
appium --chromedriver-executable /usr/local/bin/chromedriver
```

In another shell, run the Android tests.

```bash
npm run e2e-android
```

