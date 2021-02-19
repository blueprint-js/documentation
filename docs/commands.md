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

## Handling Guard Failures

When any guard in a command returns `false` from it's `passed` property in a result, an optional `fail` callback is called that
can be defined on the meta of a command within the `super` call. If this callback is not defined, the command will simply ignore
the failure and refuse to execute with no sign of failure. The `fail` gives 2 parameters, a context to the message that activated
the command, and an array of results (both failed and passing).

```ts
class NsfwCommand extends Command<BaseConfig> {
  constructor() {
    super('nsfw', {
      aliases: ['lewd', 'pics'],
      groups: ['user'],
      guards: [isNsfw],
      fail: (r, c) => {
        const res = r.filter(gr => gr.passed === false);
        c.msg.channel.createMessage(
          `Guards failed with messages: \`\`\`${res
            .map(gr => gr.message)
            .join('\n')}\`\`\``
        );
      },
    });
  }
  callback(ctx: CommandContext<BaseConfig>): void {
    // normal command stuff here
  }
}
```
