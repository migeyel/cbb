# CBB - ChatBox Brigadier
A somewhat simple and somewhat complex SwitchCraft chatbox command parser.

## Example Usage
```lua
local cbb = require "cbb"

local root = cbb.literal("pgtbox") "pgtbox" {
  cbb.literal("help") "help" {
    help = "Provides this help message",
    execute = function(ctx)
      return cbb.sendHelpTopic(1, ctx)
    end,
  },
  cbb.literal("echo") "echo" {
    cbb.string "arg1" {
      help = "Echoes a string as long as it starts with the letter 'a'",
      execute = function(ctx)
        local argument = ctx.args.arg1
        if argument:sub(1, 1):lower() == "a" then
          ctx.reply({
            text = argument,
            color = cbb.colors.AQUA,
            formats = { cbb.formats.BOLD, cbb.formats.UNDERLINE },
          })
        else
          ctx.replyErr(
            ("String %q doesn't start with 'a'"):format(argument),
            ctx.argTokens.arg1
          )
        end
      end,
    }
  },
}

while true do
    local event = { os.pullEvent("command") }
    pcall(cbb.execute, root, "pgtbox", event)
end
```
