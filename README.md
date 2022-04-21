**[Nate Flynn](https://github.com/nateflynn-silktide)**

# Webpack WordPress Plugin Version Sync

Easily sync your WordPress plugin version with your project's `package.json` file

___

## Installation

Install the package as a dev dependency

```
npm i @nateflynn/webpack-wordpress-plugin-version-sync --save-dev
```

*This package assumes that your code will run in an **ES2015+** environment.*

___

## Getting Started

Add the following requirement to your Plugin's `webpack.config.js` file:

```js
const path = require("path");
const WebpackWordPressPluginSync = require("@nateflynn/webpack-wordpress-plugin-version-sync");

module.exports = {
    plugins: [
        new WebpackWordpressPluginSync({
            configFile: path.resolve( __dirname, 'config.json' ),
            outputFile: path.resolve( __dirname, 'index.php' ),
            replacements: {
                '{VERSION}': process.env._npm_package_version
            },
            addNewlineAfter: false
        })
    ]
}
```

#### Options

| Name | Type | Default | Description |
| ---- | ---- | ------- | ----------- |
| `configFile` | `{string}` | `config.json` | Absolute path to the plugin's `config.json` file |
| `outputFile` | `{string}` | `index.php`   | The output location for the plugin's main file. By default this is the `index.php` file found at the root of your plugin directory, but this may be changed to a filename that represents your plugin name instead. |
| `replacements` | `{object}` | `{ '{VERSION}' : process.env.npm_package_version }` | An object containing key / value pairs of values to be replaced within your `config.json` file. Using this you can define your own placeholders that will be replaced when your config file is parsed. |
| `addNewlineAfter` | `{boolean}` | `false` | When set to `true` will ensure a newline is added directly after the file header when it's generated. |

#### Plugin Config

In order to compile updated headers, your plugin will need a `config.json` file. This file simply contains the header data you would usually find at the top of your plugins's main PHP file. By default this should be found at root of your plugin directory.

A typical `config.json` file will look like the following:

```json
{
    "Plugin Name": "My Awesome Plugin",
    "Plugin URI": "https://mywebsite.com",
    "Description": "A super awesome WordPress Theme that automatically keeps track of version.",
    "Version": "{VERSION}",
    "Requires at least": "5.8",
    "Requires PHP": "7.4",
    "Author": "Nate Flynn",
    "Author URI": "https://github.com/nateflynn-silktide",
    "Text Domain": "mytextdomain",
    "Domain Path": "/languages",
    "License": "GPL v3 or later",
    "License URI": "http://www.gnu.org/licenses/gpl-3.0.txt",
    "Update URI": "https://mywebsite.com/myplugin/"
}
```

If you want to add `PHPDoc DocBlock` to your plugin header, you can do so by updating the config like so:

```json
{
    "@package": "MyAwesomePlugin",
    "@author": "Nate Flynn",
    "@copyright": "2022 Nate Flynn",
    "@license": "GPL-3.0-or-later",

    "@wordpress-plugin": "",
    "Plugin Name": "My Awesome Plugin",
    "Plugin URI": "https://mywebsite.com",
    "Description": "A super awesome WordPress Theme that automatically keeps track of version.",
    "Version": "{VERSION}",
    "Requires at least": "5.8",
    "Requires PHP": "7.4",
    "Author": "Nate Flynn",
    "Author URI": "https://github.com/nateflynn-silktide",
    "Text Domain": "mytextdomain",
    "Domain Path": "/languages",
    "License": "GPL v3 or later",
    "License URI": "http://www.gnu.org/licenses/gpl-3.0.txt",
    "Update URI": "https://mywebsite.com/myplugin/"
}
```

> Notice the `Version` number has been replace with the keyword **`{VERSION}`**. Technically, this can be any string you want, this value will be replaced each time your project is rebuilt.

> **Note:** This config makes use of `PHPDoc DocBlock` file headers as well as WordPress Plugin file headers. You can add any `DocBlock` headers you need to your config as long as they appear before the `@wordpress-plugin` property.
> 
> If you *do* use `DocBloc` properties, the `@wordpress-plugin` property should be 
> 
> If you don't need `DocBlock` headers in your plugin, you can omit the `@wordpress-plugin` property from your config.