# Kitchen grocery ordering

A skill for your AI bot. You tell it what you need in chat, and it orders groceries for you, remembers what you usually buy, and keeps track of what is in your kitchen.

Say "water and bananas" and it adds the exact ones you always get. It builds the cart, shows it to you, and sends a payment link. You approve with your PIN. Later it tells you when the bananas have about a day left or the oil is running low.

## Add it to your bot

1. Fork this repo, or copy `SKILL.md`.
2. Share the skill with your bot (add it to its skills folder, or paste the link and ask it to save the skill). It will take care of the rest.
3. Connect your grocery partner when the bot asks, and sign in with your own account.

## What you need

- A bot that can use skills and connect MCP servers. It works great with Grok Bot, which can also set up the daily reminder as a routine.
- A grocery partner with an MCP server. Today that is Swiggy Instamart (India only): https://mcp.swiggy.com/builders/docs/start/what-is-swiggy-mcp/
- More partners will work as they ship MCP servers.

## What it does

- Reorders your usuals without asking "which one?" again.
- Shows numbered photo options only for new or out-of-stock items.
- Builds the cart and shows a clear table with the total.
- Leaves every payment to you: a UPI link, your wallet, or pay on delivery.
- Keeps a kitchen list with freshness estimates. Staples use Plenty, Low or Out.
- Lets you mark items finished or thrown out, and add them to your next order.
- Sends you a heads-up before food goes off or a staple runs out.

## Disclaimer

Freshness for fruits and vegetables is an estimate from typical shelf life, not a real expiry date. Check the food itself. Staples have no expiry, so they use Plenty, Low or Out instead.

## Privacy

You sign in to your own grocery account. Your addresses, orders and usuals stay in your bot's private memory and never go into this skill.
