# Plugin Registry

Blueprint uses a registry to manage plugins similar to groups, and events.
It is faily simple to register plugins, as you only need to pass the plugin
instance itself, and everything else will be handled internally. Similar to
other registries, the plugin registry can also be hooked.

## Creating Plugins

For more information on plugins and how to create them read the [plugins](../plugins.md) page.
Plugins are fairly simple to use, but due to them not being a registry in a sense, they don't fall
under the registries section, so it's best to read the page about them.

## Registering Plugins

Unlike the other registries, the plugin registry only takes a single value whern registering them, which
is the instance of the `Plugin` class itself. Everything else is handled internally by accessing the plugin's
`meta` property which is set when creating a new plugin instance.

```ts
bot.registry.plugins.register(adminPlugin);
```

Like other registries, you can unregister plugins by using the `unregister()` method with the name of the plugin.

```ts
bot.registry.plugins.unregister('admin');
```
