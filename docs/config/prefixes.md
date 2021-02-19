# Custom Prefixes

By default Blueprint only allows the use of a single prefix, defined in the `bot`
section, read more about this in the [Discord](discord.md) section. However, as of v3,
you can now setup a function to hot-load custom prefixes which are cached after the first
load to prevent heavy database load every single command call.

## Enabling Custom Prefixes

To enable custom prefixes you need to enable the `prefix` section of the Blueprint instance
options. This section contains 2 properties: `enabled` and `load`, `enabled` is a boolean which
enables or disables the use of custom prefixes, `load` is a callback used to actually fetch the
custom prefix for the guild. The `load` function only needs to be defined if you have the `enabled`
property set to `true` in the options.

```ts
const bot = new Blueprint('config.yml', {
  prefix: {
    enabled: true,
    load: (ctx) => {
      // ctx contains the message and blueprint reference
      // do what you need to here to get the prefix and return a string
    },
  }
});
```
