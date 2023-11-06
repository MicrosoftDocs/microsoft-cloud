By default, the proxy console output uses [text labels](./Console-output-text-labels).

To change the label style, update the `labelMode` property value in the [m365proxyrc.json](./m365proxyrc) file stored in your installation directory.

## ASCII icons

To use ASCII icons, use:

```json
"labelMode": "icons",
```

## Nerdfont icons

To use NerdFont icons, first ensure that you have a compatible variable-width [font installed](https://www.nerdfonts.com/font-downloads), then use:

```json
"labelMode": "nerdFont"
```
