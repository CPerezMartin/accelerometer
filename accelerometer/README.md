# Accelerometer (accelerometer)

Testing accelerometer con webpack

### Setup enviroment

Before start, check you have installed 
Node v14.19.0+
Npm v6.14.16+
Java v11.0+
Android Studio to build for android
XCode to build for iOS

At date 17/03/2023 Python for Windows system produce errors with npm. Your python version should be v3.10, don't use v3.11
In windows, verify PATH system variables for android SDK, python and Java

### Install the dependencies
```bash
yarn
# or
npm install
```

### Start the app in development mode (hot-code reloading, error reporting, etc.)
```bash
quasar dev
```


### Lint the files
```bash
yarn lint
# or
npm run lint
```


### Format the files
```bash
yarn format
# or
npm run format
```



### Build the app for production
```bash
quasar build
```

### Customize the configuration
See [Configuring quasar.config.js](https://v2.quasar.dev/quasar-cli-webpack/quasar-config-js).

### Add Android/iOS to the project
```bash
npx cap add android
# or
npx cap add iOS
```

### Open project in Android Studio or XCode
```bash
npx cap open android
# or
npx cap open iOS
```

### Convert Webview files to native
```bash
quasar build
npx cap sync 
# or
npm run build 
```