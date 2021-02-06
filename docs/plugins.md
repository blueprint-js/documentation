# Plugins

Blueprint uses a plugin system to group commands together, this also allows the ability
to allow multiple commands to share the same group(s) without having to manually specify
the groups on every single command. Plugins at the lowest level extend the `AutoRegistry`
class, and act similar to the old command registry.

## Creating Plugins

Creating a plugin is extremely simple, you just need to create a new instance of the `Plugin`
class, pasing the metadata such as the name of the plugin and optionally the groups you want
the commands inside of it to inherit.

```ts
import {Plugin} from '@dxz/blueprint';

const adminPlugin = new Plugin({
  name: 'admin',
  groups: ['admin', 'developer'],
});

adminPlugin.register(new EvalCommand());
```

## Hooking Into Plugins

Due to plugins extending the `AutoRegistry` class, they are hookable by default. This means you
can use [hooks](plugins.md) to hook into them and get debug information from them at runtime. Note
that hooking into plugins is extremely experimental and isn't garunteed to work.

```ts
const h = new Hook((r) => console.log(r));
h.bind(adminPlugin);
```
