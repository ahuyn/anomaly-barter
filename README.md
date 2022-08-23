# Anomaly NPC Bartering

This is a lightweight framework that allows for barters to be performed between the player and various NPCs.

A barter is defined as an exchange of one quantity of goods for another.

## Accessing Barters
There will be an extra button in trade UI. Click it and see what happens.

## Stipulations
These are restated in game.
- Bartering consists of 'give' and 'take' items.
- Give items are what the player receives as part of the deal.
- Take items are items that the player will hand over.
- Taken ammo must be a full box (usually 15 rounds usually for handgun and small rifle rounds, 10 for shotshells and large rifle rounds)
- Taken multiuse items (vodka, medkits) must have all uses to be considered.
- Taken items with condition must have the condition be at least 90% (adjustable in MCM)
- Taken items with parts require a minimum condition of 60% each, non-negotiable.

## Repeatability
- Some barters are subject to limitations. There are two main types:
- Fixed barters are tied to a save. Once performed, they may not be repeated.
- Restock barters refresh once the trader restocks their inventory.

# Integration Guide
1. Create a file `barter_yourcustom.ltx` under file path `configs\barter`.
2. Sample barter with fields explained.
```
[sid_jelly_suit]:sid_base
; section of the trader to register barter for
trader          = m_trader 
; item to be offered. Right now limitation is to a single instance of one item  as given reward.  Any invalid sections will render the barter invalid.
give            = stalker_outfit 
; Comma separated list, format is section,quantity. Any invalid sections will render the barter invalid.
take            = ammo_9x18_fmj,1,medkit,1,wpn_pm,1 
; These functions will be executed to check eligibility. If false is returned, the barter will be prevented. Functions are executed in order and ALL functions must return true to proceed.
precondition    = barter_core.check_limit,barter_core.check_goodwill
; This function is executed after the barter is cone.
postcondition   = barter_core.increment_limit
; Leveraged by barter_core.check_limit. This determines the type of repeatability for the barter. Valid keys are either 'fixed` or 'restock' Optional: remove for infinite repeatability.
repeat          = fixed
; Leveraged by barter_core.check_limit. Determines max trades that can be performed before the reset condition.
limit           = 1
; Leveraged by barter_core.check_goodwill. Requires the specified amount of goodwill for the given faction before allowing the barter.
goodwill        = stalker,100
```
3. Specified trader should have your item in barter menu.
4. Custom functions can be specified based oh what conditions you want your barter to show up.