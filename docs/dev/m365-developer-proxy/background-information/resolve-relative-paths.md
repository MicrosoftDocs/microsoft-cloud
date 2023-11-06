All file paths used in configuration files (`m365proxyrc.json`) are relative to the location of the configuration file.

In the below example, the `pluginPath` is relative to the configuration file. The default `m365proxyrc.json` configuration file is located in the same location where the `plugins` folder exists.

```json
{
    "name": "GraphSelectGuidancePlugin",
    "enabled": true,
    "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
    "urlsToWatch": [
        "https://graph.microsoft.com/v1.0/*",
        ...
    ]
},
```

Let's say you start the proxy from a different directory, such as from the location of a project you are working in. 

If an `m365proxyrc.json` configuration file exists in the current directory, then the file path resolution will be relative to this file. If not, the proxy will fall back to the default `m365proxyrc.json` file.

Using a configuration file that is not located in the proxy installation directory requires you to ensure that plugin paths are resolved correctly. 

Use the `~appFolder` token in the file paths to ensure that the path is prepended with the absolute path to the proxy installation directory.

```json
{
    "name": "GraphSelectGuidancePlugin",
    "enabled": true,
    "pluginPath": "~appFolder\\plugins\\m365-developer-proxy-plugins.dll",
    "urlsToWatch": [
        "https://graph.microsoft.com/v1.0/*",
        ...
    ]
},
```

The `~appFolder` token can be used in any path used by the proxy.

Let's say you want to load a preset configuration, you can use the `~appFolder` token in the path to refrence the configuration file located in the proxy installation directory.

```sh
m365proxy --config-file ~appFolder/presets/microsoft-graph.json
```