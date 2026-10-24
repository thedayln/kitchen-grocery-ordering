---
name: kitchen-grocery-ordering
description: >-
  Use this when the user asks in chat to get, order, or reorder groceries
  ("get water", "I need bananas", "the ingredients for biryani"), asks what is
  in their kitchen or what is running low, says they finished or threw
  something out, or wants reminders before food goes off. Orders through a
  grocery partner's MCP server (Swiggy Instamart today) and keeps a simple
  kitchen inventory.
---
# Kitchen grocery ordering

The user tells you what they need in chat. You order it through their grocery partner, remember what they usually buy, and keep track of what is in their kitchen so less food goes to waste. Most requests are reorders, so a known item should take one message, not five.

## 1. Connect a grocery partner

- Today this works with Swiggy Instamart's MCP server at `https://mcp.swiggy.com/im` (India only). Docs: https://mcp.swiggy.com/builders/docs/start/what-is-swiggy-mcp/
- Add it the way your bot adds MCP servers or connectors. The user signs in with their own Swiggy account through the OAuth screen. Never ask them to paste a password or token into chat.
- Other grocery apps work the same way once they offer an MCP server. Read their tool list and map it to the steps below.
- Tool names below are Instamart's: `get_addresses`, `search_products`, `your_go_to_items`, `get_cart`, `update_cart`, `get_payment_options`, `checkout`, `check_payment_status`, `confirm_order`, `track_order`. Check the server's current list before you rely on them.

## 2. First run

- Ask once which saved address to deliver to (`get_addresses`) and remember its label and id.
- Ask once how they like to pay (UPI app, wallet, or pay on delivery) and remember it.
- Read `your_go_to_items` and their past orders, if the partner offers them, to seed their usuals.

## 3. Reorder first

- Keep a usuals list: the words the user says, mapped to the exact product they buy. For example, "water" maps to one brand and size of bottled water, and "bananas" maps to one variety and count.
- When the user names a usual, add that exact product. Do not ask "which one?" again.
- Only ask when an item is new to them, their usual is out of stock, or the request is truly ambiguous. Then show 3 or 4 in-stock options as a numbered list with a photo of each (download each `imageUrl` and send the images, or one numbered photo sheet). Give brand and name, size, price (and MRP if discounted), and one short note. Ask them to reply with a number.
- Save their pick as the new usual for that word.
- If they ask about packaging or size (glass or plastic, pack of 2), check the listing name and photo, say what you can see, and say what you cannot confirm.
- Never invent products, prices, ratings or photos. Everything comes from live tool results.

## 4. Build the cart

- Call `get_cart` first, because `update_cart` replaces the whole list. Then call `update_cart` with every item (ids and quantity) and the address id.
- Show a small table: item, quantity, price, then fees, delivery, the total to pay, and the address label. Mention any free item the partner adds.
- Never place an order the user did not ask for. If they say "wait" or "hold", stop and change nothing until they say more.

## 5. Payment is always the user's approval

- Call `get_payment_options` and offer the methods they like as a short numbered choice: their UPI app (a "Pay now" link in chat opens the UPI app, where they enter their PIN), a wallet, pay on delivery, or "not yet".
- Their pick, after they have seen the cart, total and address, is their go-ahead. Call `checkout` with that exact method.
- For UPI, send the payment link (or QR) in chat and ask them to say "paid" when done. You never enter a PIN or pay on their behalf.
- When they say they paid, call `check_payment_status`. Call `confirm_order` only if that result says to. Then relay the partner's success message as it is.
- Cancelling usually cannot be done by tool. Give the support contact from the checkout result.

## 6. Keep a kitchen inventory

Store it wherever your bot keeps durable notes (memory, or a file such as `kitchen.json`). One entry per item:

- name, size or count, where it is kept (fridge, counter, pantry), date added, and either an estimated "use within" date (fresh food) or a Plenty / Low / Out state (staples).

Rules:
- After each delivered order, add the items. If the user says "I bought eggs", add them too.
- Fresh food (fruit, vegetables, milk, bread, eggs, meat, leftovers) gets an estimate from typical shelf life and where it is stored. Show it as days left and sort by what goes off first.
- Staples (rice, oil, atta, salt, sugar, onions, spices) have no expiry. They use Plenty, Low or Out, and the user can change it any time ("oil is low").
- When the user says an item is finished or thrown out, mark it, then offer "Add to next order?". Keep a next-order list and offer it at the next cart.
- When something is close to going off, you can suggest a quick way to use it with what else is in the kitchen.

## 7. Proactive reminders

Do not wait to be asked. Set up a scheduled check, daily by default (for example 9 AM in the user's time zone), with the user's OK.

- On Grok Bot, create a routine that runs this check and messages the user.
- On other bots, use whatever scheduler, cron, or reminder feature they have.

Each run, read the inventory. Send one short message only if something needs attention:
- Fresh food with about a day left: "Hey, your bananas have about a day left. Want to use them today or reorder?"
- A staple at Low or Out: "Your oil is running low. Add it to the next order?"

Group items into one message. Stay quiet when nothing needs attention, and never order from a reminder without the user saying yes.

## Disclaimer

Freshness for fruits and vegetables is an estimate from typical shelf life and storage. It is not a real expiry date, so tell the user to check the food itself. Staples have no expiry, so they use the Plenty, Low and Out state instead. Packaged goods may have a printed date, and that printed date wins.

## Privacy

- Each user connects their own grocery account. Never share one user's account, addresses, phone number, order history or payment details with anyone else, in chat, in files, or in a shared copy of this skill.
- Keep usuals, addresses and the inventory in the user's own private memory, never in this skill file.
- Never ask for or store card numbers, UPI PINs or passwords.
