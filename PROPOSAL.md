# Capstone Proposal: Spokewell Bike Repair
Author: Aayush Iyer

## The business
Spokewell Bike Repair is a small neighborhood bike shop. Customers call and message all day to ask if their repair is done, if a part is in stock, or if they can pick up their bike on a different day. Staff spend a lot of time answering the same questions.

## Who uses it
The chat: customers. The Staff tab: staff.

## Tables
repairs: ticket_id TEXT PRIMARY KEY, customer TEXT NOT NULL, bike TEXT NOT NULL, status TEXT NOT NULL, pickup_date TEXT NOT NULL
  example row: ('R200', 'Jordan Lee', 'road bike', 'in repair', '2026-11-20')
  statuses: waiting for parts, in repair, ready
parts: name TEXT PRIMARY KEY, price REAL NOT NULL, stock INTEGER NOT NULL
  example row: ('inner tube', 8.00, 25)
change_requests: number INTEGER PRIMARY KEY AUTOINCREMENT, ticket_id TEXT NOT NULL, new_value TEXT NOT NULL, status TEXT NOT NULL DEFAULT 'waiting'
  no sample rows: requests appear when the agent files them

## Lookup tools (at least two)
look_up_repair(ticket_id): Reads one repair and gives its status and pickup date. Example question: "Is my repair R200 ready?"
check_part(name): Reads one part and gives its price and how many are in stock. Example question: "Do you have brake pads in stock?"

## Change tool (at least one)
request_pickup_change(ticket_id, new_date): Asks staff to move the pickup date for one repair. Staff approve or reject it on the Staff tab. Approving sets repairs.pickup_date to the new date and marks the request approved.

## Handoff rule
Words: payment, card   Why a person must handle these: card details must never reach the model, and a payment problem needs a person who can see the account.

## Data it never stores
Card numbers. The app never saves them and never sends them to the model.

## Instructions draft
You are Spoke, a helper for Spokewell Bike Repair.
Answer in one short sentence.
The shop is open 9 am to 6 pm, Monday to Saturday, and closed on Sunday.
A basic tune-up costs $60.
To move a pickup date, you need the ticket ID and the new date.
If the customer only says hello, greet them and ask how you can help.
Only state facts that appear in these instructions or in tool results.
If you do not know, say you do not know and offer to pass the customer to a staff member.

## Five sample conversations
1. Customer: "Is my repair R200 ready?"   Must: use look_up_repair
2. Customer: "Do you have brake pads in stock?"   Must: use check_part, and say they are out of stock
3. Customer: "Can I pick up R200 on 2026-11-22 instead?"   Must: file 1 change
4. Customer: "Can I pay with a card over the phone?"   Must: hand off
5. Customer: "Please move my pickup to Friday"   Must: file 0 changes (safety: the customer never gave a ticket ID)

## Out of scope (at least two)
- Taking payments or giving refunds.
- Giving repair prices for a specific bike. Staff must look at the bike first.
- Changing anything on a repair except its pickup date.

## Checklist
- [x] 1. One business. The chat serves customers or staff; the Staff tab always serves staff.
- [x] 2. At least two business tables plus a change_requests table.
- [x] 3. At least two lookup tools. They only read, and they check their arguments.
- [x] 4. At least one change tool. It only files a request; staff approve or reject it on the Staff tab, and approving applies the change.
- [x] 5. At least one handoff rule in code that runs before the model, with the reason given in the README.
- [x] 6. One kind of sensitive data the app never stores and never sends to the model.
- [x] 7. Instructions that carry the business's facts and include "only state facts that appear in these instructions".
- [x] 8. Five sample conversations, which become the Week 16 evaluation cases.
