KB-Chat Desktop
===============

KB-Chat Desktop is a Matrix client for desktop platforms with KB-Chat Web at its core.

First Steps
===========
Before you do anything else, fetch the dependencies:

```
yarn install
```

Fetching KB-Chat
================
Since this package is just the Electron wrapper for KB-Chat Web, it doesn't contain any of the KB-Chat Web code,
so the first step is to get a working copy of KB-Chat Web. There are a few ways of doing this:

```
# Fetch the prebuilt release KB-Chat package from the kb-chat-web GitHub releases page. The version
# fetched will be the same as the local KB-Chat-desktop package.
# We're explicitly asking for no config, so the packaged KB-Chat will have no config.json.
yarn run fetch --noverify --cfgdir ""
```

...or if you'd like to use GPG to verify the downloaded package:
```
# Fetch the KB-Chat public key from the chat.klabausterbeere.xyz web server over a secure connection and import
# it into your local GPG keychain (you'll need GPG installed). You only need to to do this
# once.
yarn run fetch --importkey
# Fetch the package and verify the signature
yarn run fetch --cfgdir ""
```

...or either of the above, but fetching a specific version of KB-Chat:
```
# Fetch the prebuilt release Element package from the KB-Chat-web GitHub releases page. The version
# fetched will be the same as the local KB-Chat-desktop package.
yarn run fetch --noverify --cfgdir "" v1.5.6
```

If you only want to run the app locally and don't need to build packages, you can
provide the `webapp` directory directly:
```
# Assuming you've checked out and built a copy of KB-Chat-web in ../kb-chat-web
ln -s ../kb-chat-web/webapp ./
```

[TODO: add support for fetching develop builds, arbitrary URLs and arbitrary paths]

Building
========

## Native Build

TODO: List native pre-requisites

Optionally, [build the native modules](https://github.com/bkarich/kb-chat-desktop/blob/develop/docs/native-node-modules.md), 
which include support for searching in encrypted rooms and secure storage. Skipping this step is fine, you just won't have those features.  

Then, run
```
yarn run build
```
This will do a couple of things:
 * Run the `setversion` script to set the local package version to match whatever
   version of KB-Chat you installed above.
 * Run electron-builder to build a package. The package built will match the operating system
   you're running the build process on.

## Docker

Alternatively, you can also build using docker, which will always produce the linux package:
```
# Run this once to make the docker image
yarn run docker:setup

yarn run docker:install
# if you want to build the native modules (this will take a while)
yarn run docker:build:native
yarn run docker:build
```

After running, the packages should be in `dist/`.

Starting
========
If you'd just like to run the electron app locally for development:
```
# Install electron - we don't normally need electron itself as it's provided
# by electron-builder when building packages
yarn add electron
yarn start
```

Config
======
If you'd like the packaged KB-Chat to have a configuration file, you can create a
config directory and place `config.json` in there, then specify this directory
with the `--cfgdir` option to `yarn run fetch`, eg:
```
mkdir myconfig
cp /path/to/my/config.json myconfig/
yarn run fetch --cfgdir myconfig
```
The config dir for the official KB-Chat app is in `klabausterbeere.xyz`. If you use this,
your app will auto-update itself using builds from klabausterbeere.xyz.

Profiles
========

To run multiple instances of the desktop app for different accounts, you can
launch the executable with the `--profile` argument followed by a unique
identifier, e.g `kb-chat-desktop --profile Work` for it to run a separate profile and
not interfere with the default one.

Alternatively, a custom location for the profile data can be specified using the
`--profile-dir` flag followed by the desired path.

User-specified config.json
==========================

+ `%APPDATA%\$NAME\config.json` on Windows
+ `$XDG_CONFIG_HOME\$NAME\config.json` or `~/.config/$NAME/config.json` on Linux
+ `~/Library/Application Support/$NAME/config.json` on macOS

In the paths above, `$NAME` is typically `Element`, unless you use `--profile
$PROFILE` in which case it becomes `Element-$PROFILE`, or it is using one of
the above created by a pre-1.7 install, in which case it will be `Riot` or
`Riot-$PROFILE`.

Translations
==========================

To add a new translation, head to the [translating doc](https://github.com/bkarich/kb-chat-web/blob/develop/docs/translating.md).

For a developer guide, see the [translating dev doc](https://github.com/bkarich/kb-chat-web/blob/develop/docs/translating-dev.md).

[<img src="https://translate.element.io/widgets/element-desktop/-/multi-auto.svg" alt="translationsstatus" width="340">](https://translate.element.io/engage/element-desktop/?utm_source=widget)

Report bugs & give feedback
==========================

If you run into any bugs or have feedback you'd like to share, please let us know on GitHub.

To help avoid duplicate issues, please [view existing issues](https://github.com/bkarich/kb-chat-web/issues?q=is%3Aopen+is%3Aissue+sort%3Areactions-%2B1-desc) first (and add a +1) or [create a new issue](https://github.com/bkarich/kb-chat-web/issues/new/choose) if you can't find it.  Please note that this issue tracker is associated with the [element-web](https://github.com/bkarich/kb-chat-web/) repo, but is also applied to the code in this repo as well.
