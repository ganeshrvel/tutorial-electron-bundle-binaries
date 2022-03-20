# Bundle a precompiled binary or native file into an electron app.

- Author: [Ganesh Rathinavel](https://www.linkedin.com/in/ganeshrvel "Ganesh Rathinavel")
- License: [MIT](https://github.com/ganeshrvel/tutorial-electron-bundle-binaries/blob/master/LICENSE "MIT")
- Website URL: [https://github.com/ganeshrvel/tutorial-electron-bundle-binaries](https://github.com/ganeshrvel/tutorial-electron-bundle-binaries/ "https://github.com/ganeshrvel/tutorial-electron-bundle-binaries")
- Repo URL: [https://github.com/ganeshrvel/tutorial-electron-bundle-binaries](https://github.com/ganeshrvel/tutorial-electron-bundle-binaries/ "https://github.com/ganeshrvel/tutorial-electron-bundle-binaries")
- Contacts: ganeshrvel@outlook.com


### Introduction

##### Bundling the pre-compiled native files into an ElectronJs app could get tricky as asar packager tends to exclude such files while packaging the app. I have spent a significant amount of time researching how to get this done. This was originally implemented inside [OpenMTP - Advanced Android File Transfer Application for macOS](https://github.com/ganeshrvel/openmtp "OpenMTP - Advanced Android File Transfer Application for macOS").

### Implementation

- Create the OS folder inside your electron "build" directory.

```shell
# Replace {OS} with the required OS name. eg: mac, win, linux
$ mkdir -p ./build/{OS}/bin
```

- Place the binaries inside this folder.
- Install npm packages

```shell
$ npm install electron-root-path

or 

$ yarn add electron-root-path
```

- Create the file *./app/binaries.js* and add the below code

```javascript
'use strict';

import path from 'path';
import { remote } from 'electron';
import { rootPath } from 'electron-root-path';
import getPlatform from './get-platform';

const IS_PROD = process.env.NODE_ENV === 'production';
const root = rootPath;
const { getAppPath } = remote.app;
const isPackaged =
  process.mainModule.filename.indexOf('app.asar') !== -1;

const binariesPath =
  IS_PROD && isPackaged
    ? path.join(path.dirname(getAppPath()), '..', './Resources', './bin')
    : path.join(root, './resources', getPlatform(), './bin');

export const execPath = path.resolve(path.join(binariesPath, './binary-file'));
```

- Create the file *./app/get-platform.js* and paste the below code

```javascript
'use strict';

import { platform } from 'os';

export default () => {
  switch (platform()) {
    case 'aix':
    case 'freebsd':
    case 'linux':
    case 'openbsd':
    case 'android':
      return 'linux';
    case 'darwin':
    case 'sunos':
      return 'mac';
    case 'win32':
      return 'win';
  }
};
```

- Add the below code to your *./package.json* file

```javascript
"build": {
 "extraFiles": [
      {
        "from": "build/mac/bin",
        "to": "Resources/bin",
        "filter": [
          "**/*"
        ]
      }
    ],
},
```

- To distribute the code via Mac App Store add the below code to your *package.json* file

```javascript
"build": {
	"mas": {
      "type": "distribution",
      "category": "public.app-category.productivity",
      "entitlements": "build/entitlements.mas.plist",
      "icon": "build/icon.icns",
      "binaries": [
        "dist/mas/<APP_NAME>.app/Contents/Resources/bin/mtp-cli"
      ]
    },
}
```


- Import the binary file as:

```javascript
import { execPath } from './binaries';

// your program code:
var command = spawn(execPath, arg, {});
```


### Clone
```shell
$ git clone --depth 1 --single-branch --branch master https://github.com/ganeshrvel/tutorial-electron-bundle-binaries.git

$ cd tutorial-electron-bundle-binaries
```

### Contribute
- Fork the repo and create your branch from master.
- Ensure that the changes pass linting.
- Update the documentation if needed.
- Make sure your code lints.
- Issue a pull request!

When you submit code changes, your submissions are understood to be under the same [MIT License](https://github.com/ganeshrvel/tutorial-electron-bundle-binaries/blob/master/LICENSE "MIT License") that covers the project. Feel free to contact the maintainers if that's a concern.


### Buy me a coffee
Help me keep the app FREE and open for all.
Paypal me: [paypal.me/ganeshrvel](https://paypal.me/ganeshrvel "paypal.me/ganeshrvel")

### Contacts
Please feel free to contact me at ganeshrvel@outlook.com

### More repos
- [OpenMTP  - Advanced Android File Transfer Application for macOS](https://github.com/ganeshrvel/openmtp "OpenMTP  - Advanced Android File Transfer Application for macOS")
- [Tutorial Series by Ganesh Rathinavel](https://github.com/ganeshrvel/tutorial-series-ganesh-rathinavel "Tutorial Series by Ganesh Rathinavel")
- [npm: electron-root-path](https://github.com/ganeshrvel/npm-electron-root-path "Get the root path of an Electron Application")
- [Electron React Redux Advanced Boilerplate](https://github.com/ganeshrvel/electron-react-redux-advanced-boilerplate "Electron React Redux Advanced Boilerplate")

### License
tutorial-electron-bundle-binaries is released under [MIT License](https://github.com/ganeshrvel/tutorial-electron-bundle-binaries/blob/master/LICENSE "MIT License").

Copyright © 2018-Present Ganesh Rathinavel
