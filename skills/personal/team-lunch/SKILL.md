---
name: team-lunch
description: Order lunch (or any meal) for the team on DoorDash with dd-cli, taking the full order — cuisine, restaurant, headcount, dietary needs — and placing it. Use when the user says they are hungry, says "order lunch" or "order food", or asks to feed the team while working.
allowed-tools: Bash
---

# Team Lunch

Take a team food order end to end with `dd-cli`: from "we're hungry" to a placed
DoorDash order, sized for everyone, covering every diet, and billed to a work
budget when one exists.

`dd-cli` must be on PATH and signed in (`dd-cli login`). For the command tree and
its options, follow the `dd-cli-usage` skill. Run every data command with
`--json-output`; render the price preview with `--beautify`.

## 1. Take the brief

Ask only for what the conversation hasn't already given. Gather in one or two
turns:

- **Headcount** — how many people are eating.
- **Cuisine or restaurant** — a craving, a type, or a named place.
- **Dietary needs** — veg, vegan, halal, Jain, allergies. Always ask; a team of
  any size usually has at least one constraint.
- **Budget** — per head or total, if there is one.
- **Deliver-to and by-when** — the office and the time it must arrive.

## 2. Resolve the delivery address

Run `dd-cli --json-output address list`. Saved addresses may carry no label, so
do not guess which is the office — show the candidates and confirm the one where
the team is working. Use that entry's `lat` / `lng` for the search. If it reads
as a workplace, treat that as a work-benefits signal for step 6.

## 3. Find an orderable restaurant

Run `dd-cli --json-output search -q "<cuisine>" --lat <lat> --lng <lng>`. Drop
any store where `is_link_out` is true — `dd-cli` cannot check those out. Offer
two or three that fit the cuisine, the dietary needs, the rating, and an ETA that
beats the deadline. Let the user pick.

## 4. Build the spread

Run `dd-cli --json-output menu --store-id <id>`. Propose a spread sized for the
headcount that covers every dietary need — about one main per person plus a few
shareables, inside the budget. List the items with prices, get approval, and
adjust. For any item with a size or modifier choice, confirm the options with
`dd-cli --json-output restaurant-item-details --store-id <id> --menu-id <mid>
--item-id <iid>` before adding it.

## 5. Fill the cart

Before starting a new cart, run `dd-cli --json-output cart list --store-id <id>`
and surface any cart already open at that store — offer to extend it or delete it
and start fresh. Add items with `dd-cli --json-output cart add-items --menu-id
<mid> --items-json '[...]'`, passing each chosen option as `nested_options`. Keep
the returned `cart_uuid` and pass it on every later call.

## 6. Preview with work benefits

A team lunch is a work-benefits signal, so always preview with the flag:

```
dd-cli order preview --cart-uuid <uuid> --include-work-benefits --beautify
```

Show the `--beautify` output verbatim. Filter
`quote.expense_order_options.all_eligible_expense_order_budgets[]` to entries
whose remaining amount is above zero. If any exist, name them with their
remaining balance and ask which to apply — never apply silently — then re-preview
with `--selected-budget-id <id>`. If the chosen budget needs an expense code or a
note, collect them. If none are eligible, do not mention budgets; the default
card pays.

## 7. Confirm and place

Before submitting, confirm three things with the user: the total, the Dasher tip
for a delivery, and the payment method — the applied budget, or a card named by
brand and last four from `dd-cli --json-output payment-method list`. Submit only
on an explicit yes:

```
dd-cli order submit --cart-uuid <uuid> --tip-cents <n> [--selected-budget-id <id> --team-id <id>]
```

Pass the tip in cents. When a budget is applied, pass its `team_id` as
`--team-id`. Treat the cart as spent once submit returns — never re-submit the
same `cart_uuid`. If the preview flags a `PIN_CODE` dropoff, tell the user to
read the PIN off the tracking page for the Dasher. If they would rather finish in
a browser, hand off `dd-cli order checkout-url --cart-uuid <uuid>` instead of
submitting.

Done when the order is submitted — report the confirmation, what was ordered, and
for how many — or when the user stops.
