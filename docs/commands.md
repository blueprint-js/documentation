# Commands

Blueprint uses a `Command` class to manage the implementation of commands, for things such as the command's
meta, a `super()` call with the meta of the command, such as the name, groups, aliases, and guards must be used.
An example of creating a command can be seen below in the **Creating Commands** section. Note that this is a breaking
change from the old pre-2.4.0 decorator method.

## Guards

Guards a way to add pre-checks to commands. They are simply functions that take the message context and a reference to
the blueprint instance and return a `GuardResult` type. In order for a command using guards to execute, **all guards must pass**.

```ts
import {Guard, BaseConfig} from '@dxz/blueprint';
import {GuildChannel} from 'eris';

const isNsfw: Guard<BaseConfig> = ctx => {
  if ((ctx.channel as GuildChannel).nsfw) return {passed: true};
  else return {passed: false, message: "The channel is not marked as NSFW"};
};
```

## Creating Commands

Commands are classes extending the abstract `Command` class, to set up a command you need to use the `super()` method to assign
the command's meta information, then implement the abstract `callback` method with the functionality of the command.

```ts
export class PingCommand extends Command {
  constructor() {
    super('ping', {
      aliases: ['latency', 'lag'],  
      groups: ['user'],
      guards: [],
    });
  }
  async callback(ctx: CommandContext<BaseConfig>) {
    const startTime = Date.now();
    const msg = await ctx.message.channel.createMessage('loading...');
    msg.edit('The current ping is `' + (Date.now() - startTime) + 'ms`');
  }
}
```
