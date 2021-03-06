# Electron

run the electron version:

```bash
electron .
```

## File System

When using ***Electron***, aurelia-fetch-client calls that attempt to retrieve files from the current URL for the app fail because they start with `file://`.

The fix is to detect this case and use the node `fs` object to access the file system.

`window.__dirname` has the path to the app resources for both `electron .` and when running the installed app.

## Electron Packaging and Installation

to run the electron packaging command and then generate the installer, run the npm command:

```bash
npm run build
```

the `scripts` property in `package.json`:

```javascript
  "scripts": {
    "clean": "rimraf dist",
    "start": "electron .",
    "exe32": "electron-packager . plotterplatform --platform win32 --arch ia32 --out dist/ --ignore \"(dist|node_modules)\"",
    "exe64": "electron-packager . plotterplatform --platform win32 --arch x64 --out dist/ --ignore \"(dist|node_modules)\"",
    "set32": "electron-installer-windows --src dist/plotterplatform-win32-ia32/ --dest dist/installers/ia32/",
    "set64": "electron-installer-windows --src dist/plotterplatform-win32-x64/ --dest dist/installers/x64/",
    "build": "npm run clean && npm run exe32 && npm run set32",
    "build-all": "npm run clean && npm run exe32 && npm run set32 && npm run exe64 && npm run set64"
  },
```

the installation will be in `dist/installers/ia32`.

doesn't seem to update the apps in windows, but if you run the `setup.exe` it launches the app.

some notes follow:

### Run the packaging command:

`electron-packager . ppApp --platform win32 --arch ia32 --out electron/ --overwrite`

### Generate the installer

`npm install -g electron-installer-windows`

`npm install --save-dev electron-installer-windows`

`eAppConfig.json`

```javascript
{
    "src": "c:\a\p\platform\electron\ppApp-win32-ia32",
    "dest": "c:\a\p\platform\eApp\PlotterPlatform\",
    "authors": ["cmichaelgraham"]
}
```

`electron-installer-windows --config eAppConfig.json`

### Electron Packager Help

`electron-packager --help`

```bash
$ electron-packager --help
Usage: electron-packager <sourcedir> <appname> --platform=<platform> --arch=<arch>

Required options

sourcedir          the base directory of the application source

  Either both of:

platform           all, or one or more of: darwin, linux, mas, win32 (comma-delimited if multiple)
arch               all, or one or more of: ia32, x64 (comma-delimited if multiple)

  Or:

all                equivalent to --platform=all --arch=all

  Examples:        electron-packager ./ --platform=darwin --arch=x64
                   electron-packager ./ --all

Optional options

appname            the name of the app, if it needs to be different from the "productName" or "name"
                   in the nearest package.json

* All platforms *

app-copyright      human-readable copyright line for the app
app-version        release version to set for the app
asar               whether to package the source code within your app into an archive. You can either
                   pass --asar by itself to use the default configuration, or use dot notation to
                   configure a list of sub-properties, e.g. --asar.unpackDir=sub_dir

                   Properties supported:
                   - ordering: path to an ordering file for file packing
                   - unpack: unpacks the files to the app.asar.unpacked directory whose filenames
                     regex .match this string
                   - unpackDir: unpacks the dir to the app.asar.unpacked directory whose names glob
                     pattern or exactly match this string. It's relative to the <sourcedir>.
asar-unpack        unpacks the files to the app.asar.unpacked directory whose filenames regex .match
                   this string
                   (Deprecated, use asar.unpack instead)
asar-unpack-dir    unpacks the dir to the app.asar.unpacked directory whose names glob pattern or
                   exactly match this string. It's relative to the <sourcedir>.
                   (Deprecated, use asar.unpackDir instead)
build-version      build version to set for the app
cache              directory of cached Electron downloads. Defaults to `$HOME/.electron`
                   (Deprecated, use --download.cache instead)
deref-symlinks     whether symlinks should be dereferenced. Defaults to true.
download           a list of sub-options to pass to electron-download. They are specified via dot
                   notation, e.g., --download.cache=/tmp/cache
                   Properties supported:
                   - cache: directory of cached Electron downloads. Defaults to `$HOME/.electron`
                   - mirror: alternate URL to download Electron zips
                   - strictSSL: whether SSL certs are required to be valid when downloading
                     Electron. Defaults to true, use --download.strictSSL=false to disable checks.
icon               the icon file to use as the icon for the app. Note: Format depends on platform.
ignore             do not copy files into app whose filenames regex .match this string. See also:
                   https://github.com/electron-userland/electron-packager/blob/master/docs/api.md#ignore
                   and --prune.
out                the dir to put the app into at the end. defaults to current working dir
overwrite          if output directory for a platform already exists, replaces it rather than
                   skipping it
prune              runs `npm prune --production` on the app
strict-ssl         whether SSL certificates are required to be valid when downloading Electron.
                   It defaults to true, use --strict-ssl=false to disable checks.
                   (Deprecated, use --download.strictSSL instead)
tmpdir             temp directory. Defaults to system temp directory, use --tmpdir=false to disable
                   use of a temporary directory.
version            the version of Electron that is being packaged, see
                   https://github.com/electron/electron/releases

* darwin/mas target platforms only *

app-bundle-id      bundle identifier to use in the app plist
app-category-type  the application category type
                   For example, `app-category-type=public.app-category.developer-tools` will set the
                   application category to 'Developer Tools'.
extend-info        a plist file to append to the app plist
extra-resource     a file to copy into the app's Contents/Resources
helper-bundle-id   bundle identifier to use in the app helper plist
osx-sign           (OSX host platform only) Whether to sign the OSX app packages. You can either

                   pass --osx-sign by itself to use the default configuration, or use dot notation
                   to configure a list of sub-properties, e.g. --osx-sign.identity="My Name"
                   Properties supported:
                   - identity: should contain the identity to be used when running `codesign`
                   - entitlements: the path to entitlements used in signing
                   - entitlements-inherit: the path to the 'child' entitlements

* win32 target platform only *

version-string     a list of sub-properties used to set the application metadata embedded into
                   the executable. They are specified via dot notation,
                   e.g. --version-string.CompanyName="Company Inc."
                   or --version-string.ProductName="Product"
                   Properties supported:
                   - CompanyName
                   - FileDescription
                   - OriginalFilename
                   - ProductName
                   - InternalName```

## Here's an example command:

```bash
ZIK51127@ENGLTGC7NFC2 MINGW64 /c/a/e1/electron-quick-start (master)
$ electron-packager . electron-quick-start --all
Downloading electron-v1.3.1-linux-ia32.zip
Downloading electron-v1.3.1-win32-ia32.zip
Downloading electron-v1.3.1-darwin-x64.zip
Downloading electron-v1.3.1-linux-x64.zip
Downloading electron-v1.3.1-mas-x64.zip
[============================================>] 100.0% of 42.3 MB (3.6 MB/s)
Packaging app for platform mas x64 using electron v1.3.1
WARNING: signing is required for mas builds. Provide the osx-sign option, or manually sign the app later.
Packaging app for platform win32 x64 using electron v1.3.1
Wrote new apps to:
C:\a\e1\electron-quick-start\electron-quick-start-linux-ia32
C:\a\e1\electron-quick-start\electron-quick-start-win32-ia32
C:\a\e1\electron-quick-start\electron-quick-start-darwin-x64
C:\a\e1\electron-quick-start\electron-quick-start-linux-x64
C:\a\e1\electron-quick-start\electron-quick-start-mas-x64
C:\a\e1\electron-quick-start\electron-quick-start-win32-x64
```