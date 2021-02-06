# Hooks

Hooks are a way to get debugging information from registries inside of Blueprint. All
registries extend a `Hookable` class which implements the hook implementation which adds
a `hook` and `unhook` method. While you can manually use this to bind hooks, it is recommended
to use the `Hook` class to manage hooks.

## Binding Hooks

The act of 'binding' a hook binds a callback to a registry to get debug information this is done
via the `Hook` class, in an instance, an example of how to create and bind hooks can be seen below, 
note that hooking a lot of things can and will spam your console with messages and data if you log everything.

```ts
import {Blueprint, Hook} from '@dxz/blueprint';

const bot = new Blueprint('config.json');

const hook = new Hook((res) => console.log(res.message));
hook.bind(bot.registry.events, bot.registry.groups);
bot.start();
```

### Unbinding Hooks

Unbinding hooks is the act of removing the callback or "hook" from registries, the hook class makes this easy by
allowing you to use the `.unbind()` method which takes a list of the hooks you want to unbind.

```ts
hook.unbind(bot.registry.events, ...);
```
