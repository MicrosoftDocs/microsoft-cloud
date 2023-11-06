We provide a number of preset configurations that can be used to recreate common scenarios against known APIs.

This make it easier to load a known configuration without having to configure a settings file.

The configurations are stored in the presets folder in the proxy installation directory.

To use a preset, use the `--config-file` option and pass the path to the configuration file:

```sh
m365proxy --config-file presets/microsoft-graph-rate-limiting.json
```