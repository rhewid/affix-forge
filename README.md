# Affix Forge

A mod for [Ragnarok Offline](https://github.com/Flux159/ragnarokoffline.app) that adds an affix system to the gear you already have.

- **Affix drops.** Equipment that monsters can drop also comes in a version with **1-4 random affixes** (bonus attributes), picked from **214 options in four rarity tiers**. No new items, no chests: the rolls happen on the monster's own drops.
- **The Affix Smith** (Prontera, 164,176) moves all or just one affix from one item to another, removes a chosen affix, or rerolls an item's affixes, for zeny and materials **you set**.

It works on stock items in renewal and pre-renewal, adds no custom map, and needs Ragnarok Offline 1.4.3 or newer.

## Install

1. Copy the `affix-forge` folder to `%APPDATA%\Ragnarok Offline\state\mods`, or use **Settings → Mods → Add mod from folder**.
2. Switch it on in Settings → Mods and press **Apply**.
3. Restart the game client once (it caches files by name; this loads the affix names).

## How the drops work

Whenever you kill a monster, each weapon, armor or shadow gear piece in **its drop list** gets an extra roll at its normal drop rate (times the "Modded drop chance" setting). A win gives you a copy of that item with 1-4 distinct affixes.

Each affix is rolled in two steps: a **rarity tier** by weight, then a random option from that tier that fits the item (weapon-only and armor-only options respect the item type).

| Tier | Default weight | What is in it |
|---|---|---|
| 1 Common | 55 | STR/AGI/..., ATK, MATK, flat HP/SP, DEF/MDEF, HIT, FLEE, CRIT, recovery speed |
| 2 Uncommon | 28 | HP/SP %, ATK/MATK %, ASPD, cast time and delay, SP cost, healing, element/race/size resistances, damage reduction from monster elements |
| 3 Rare | 14 | Damage vs race/element/size, magic damage by element, crit damage, unbreakable gear |
| 4 Epic | 3 | Boss damage, Ignore DEF/MDEF, bonus EXP, weapon and armor element conversion |

Values scale with the monster's level (switchable), every option has a ceiling, and a **value multiplier** setting scales them all. With **One affix per stat** on (the default) an item never gets, for example, both MaxHP and MaxHP %.

**Ground drop mode** (off by default): instead of going into your inventory identified, the item drops on the ground next to you, **unidentified, with a pillar of light** like a card drop. The pillar is blue for 1 affix, yellow for 2, purple for 3-4. Identify it after picking it up to see the affixes. (If dropped items don't show up in your client, turn the pillar off.)

**Autoloot:** in ground drop mode, if your `@autoloot` would have picked up the stock drop (its rate is within your autoloot rate and its type is allowed, or it is on your `@autolootitem` list), the affixed item goes straight into your inventory, still unidentified, instead of onto the ground.

The stock drop of the same item still happens as before; the affix version is a separate roll.

**Why the plain item still drops:** the affixes are stored on the item when the server creates it, and the stock drop is created and placed (or autolooted) before any script runs. The kill event is the first point a script can act, and by then the plain item already exists. No script command can add options to an existing item, or reliably find and remove one lying on the ground, so the mod can't turn the stock drop into an affixed one. It rolls a separate affixed copy instead. Making the affixed item replace the plain one needs a change in the rAthena fork itself, for example a hook that adds random options as the drop is created.

## The Affix Smith

Stand in front of him in Prontera at **164,176**. He only touches **unequipped** equipment. Every service shows a preview of what will be taken and what you get before anything is spent.

| Service | What happens |
|---|---|
| **Move ALL affixes** | Choose an item with affixes (it is **used up**) and any other equipment or costume. The target gets the source's affixes exactly as they were, replacing its own. |
| **Move ONE affix** | Choose an item (it is **used up**), choose one of its affixes, then the item to put it on. It takes a free slot; if the target already has that affix its value is replaced; if it has five already, you choose which to give up. |
| **Remove an affix** | Choose an item and one affix. It is removed; the item stays. |
| **Reroll** | Choose an item. It keeps the same *number* of affixes, each rolled anew from the same pool and weights as the drops. Values scale with the item's required level. |

The target always keeps its refine, cards, enchants, grade and bound state.

### Costs

Each service costs **zeny and up to 3 material entries**, all in Settings → Mods. A materials setting is up to 3 entries separated by commas, each one of:

- `ITEMID:AMOUNT`, an Etc or usable item, for example `7321:5`
- `same:AMOUNT`, that many **plain** pieces of equipment of the same kind as the item being worked on: the same weapon type (daggers for a dagger), or the same equipment slot

Example: `7321:5,same:2`. *Plain* means not equipped, no refine, no cards or enchants, no grade and no affixes, so the Smith can never use up something valuable by accident; the cheapest matching pieces go first. Equipment ids are not accepted as a normal item entry (use `same`), since taking one by id could take a refined copy. Leave a materials setting empty for none.

## Settings

| Setting | Default |
|---|---|
| Modded drop chance (% of the monster's drop rate; 0 = no drops) | 100 |
| Minimum / maximum affixes per item | 1 / 4 |
| Scale values with monster level | on |
| One affix per stat | on |
| Drop on the ground (unidentified, with a pillar) | off |
| Ground drop: show the pillar of light | on |
| Rarity weight, tiers 1 / 2 / 3 / 4 | 55 / 28 / 14 / 3 |
| Value multiplier (%) | 100 |
| Move all affixes: price / materials | 100000 / none |
| Move one affix: price / materials | 60000 / none |
| Remove an affix: price / materials | 30000 / none |
| Reroll: price / materials | 50000 / none |

Press **Apply** after changing settings; the server restarts.

## How it differs from ARPG Equipments Mod

[ARPG Equipments Mod](https://github.com/igueradx/ARPG-Equipments-Mod) by Iguera was the inspiration. Both give gear random options, but they work differently:

| | ARPG Equipments Mod | Affix Forge |
|---|---|---|
| How you get affixed gear | Six tiered **Equipment Chests** drop by monster level; opening one gives a piece of gear | **Monsters drop the gear itself** with affixes, from their own drop list; no chests |
| Rolls | 4 options, one from each of four fixed categories (basic, offensive, defensive, utility), small ranges | **1-4** affixes (you choose the range) from **214 options in four rarity tiers**, weights and a value multiplier adjustable, scaled by monster level |
| Duplicates | Fixed category slots | Never the same affix twice; optional **one affix per stat** |
| Seeing the drop | Items go to your inventory | Inventory, **or** on the ground unidentified with a pillar of light |
| Crafting | A hub map with a Dismantler, Bonus Stats Extractor (Option Scrolls) and Crafter, paid in Equipment Essence | **The Affix Smith** in Prontera: move all, move one, remove, reroll, paid in zeny and materials **you configure** |
| New content | Custom map, custom items (chests, essences, scrolls) | **None**: no new items, no map; works on stock items |
| Drop rates | Per-chest rates, with an in-game customizer NPC | Drop chance as a percentage of each monster's own rate |

They are different designs, not drop-in replacements, and I have not tested running both together. If you do, expect two sets of random-option gear in your inventory and two separate crafting systems.

## Editing the option pool

Open `npc/affix_drops.txt`. Every option is one row in `OnInit`:

```
callsub L_Add, <option id>, <tier 1-4>, <min>, <max>, <kind>, <item type>, <cap>, <group>;
```

Change a row's tier, range or cap, delete a row to remove an option, or copy one to add any option from rAthena's `item_randomopt_db.yml`. The column meanings are in the comment above the rows. The Smith's reroll reads the same pool, so one edit covers both. Names shown in game come from `data/luafiles514/lua files/datainfo/addrandomoptionnametable.lub`, which already names every option id.

## Known limits

- The Smith moves affixes without checking the one-affix-per-stat rule, so a moved affix can create a pair like MaxHP and MaxHP % on one item.
- Drops are an extra roll; the stock drop of the same item is unaffected.
- The Affix Smith is newer than the drops and has had less play-testing. Please report problems.

## Credits and licenses

- Code: MIT, see `LICENSE`.
- Concept inspired by [ARPG Equipments Mod](https://github.com/igueradx/ARPG-Equipments-Mod) (MIT, Copyright 2026 Igor / Iguera). The drop and option-pool code is original. The Smith reads an item's random options and recreates the item the same way that mod's Bonus Stat Crafter does, so its MIT notice is reproduced in `LICENSE`.
- Option names in `addrandomoptionnametable.lub` come from the [ROenglishRE](https://github.com/llchrisll/ROenglishRE) translation by zackdreaver and llchrisll, which may be used and modified freely.
- Built for Ragnarok Offline by Flux159 and runs on rAthena.
