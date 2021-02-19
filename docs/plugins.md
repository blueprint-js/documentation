# Plugins

Blueprint uses a plugin system to group commands together, this also allows the ability
to allow multiple commands to share the same group(s) without having to manually specify
the groups on every single command. Plugins act simply as groups and can apply permissions
to multiple commands by passing the permission groups to the plugin.

## Creating Plugins

Creating a plugin is extremely simple, you just need to create a new instance of the `Plugin`
class, pasing the metadata such as the name of the plugin and optionally the groups you want
the commands inside of it to inherit. As of version 3.0.0, commands are now passed into the constructor
of plugins inside the meta object.

```ts
import {Plugin} from '@dxz/blueprint';

const adminPlugin = new Plugin({
  groups: ['admin', 'developer'],
  commands: [new EvalCommand()],
  name: 'admin',
});
```
