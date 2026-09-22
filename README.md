# Skript_Item_Designer
A Skript Item Designer to help create custom items in Minecraft using the Skript Plugin. 

A single-file browser tool for teaching custom item development with [Skript](https://github.com/SkriptLang/Skript) on a Paper Minecraft server. Students design a four-item set, watch the Skript write itself, and playtest the behaviour against a simulated server before touching real code.

Built for a classroom unit. No install, no accounts, no server required to use it.

## What it does

**Design** — name an item, pick a base Minecraft type from ~70 grouped options, choose one of 14 triggers and one of 30 effects, then set charges and a cooldown. The design is restated as a plain-English sentence so students can check that their code says what their idea said.

**Generate** — produces ready-to-run Skript for the whole set: a single give function, a player command, an optional admin command, and one event block per item. The generator writes the patterns beginners get wrong:

- `uncolored name` checks, so a stray colour code can't bypass the item check
- variables keyed by UUID, so they survive a player name change
- timestamp cooldowns (`difference between … and now`) instead of `wait` + `delete`, which strands players when the server restarts mid-wait
- a target lookup with a guard clause when the chosen effect needs a target the event doesn't provide
- the right inventory slot per base type — `player's chestplate` for armour, not `player's tool`
- `loop all players:` with `continue` guards for passive timer items, because `stop` inside a loop kills the trigger for everyone else

Comment lines mark the places the student still has to think for themselves.

**Playtest** — a fake server with a zombie that has real health, a range toggle, status effects that tick, player buffs with countdowns, and a hotbar. Pressing an action that doesn't match the item's trigger correctly does nothing, which is the point. A Console view shows the plausible Skript reload output for the current design, including the errors it would actually throw.

**Deploy** — a pre-flight check that lints the design live (armour on a hand-only event, eating a diamond sword, duplicate item names sharing charges, curly quotes from a word processor, an effect that's still a comment), the exact save-and-reload sequence with the student's own filename, and a `Hand in design` button that downloads their work as a JSON file for collection.

## For students

1. Open the page. Fill in Item 1 — name, base type, what triggers it, what it does.
2. Read the plain-English sentence. If it isn't what you meant, fix the design, not the code.
3. Test it in the Playtest panel. Press the action that matches your trigger, then press one that doesn't.
4. Do the same for items 2–4. Two are required.
5. Clear the pre-flight check, then follow the deploy steps.
6. Press **Hand in design** and upload the file that downloads.

Work saves automatically in the browser, so a refresh won't lose it. On a shared computer, press **Start a new set** before you begin so you don't inherit the last person's work.

## For teachers

Requires **Skript 2.9+ on Paper**. The generated scripts use no addons.

Suggested progression: design in the tool → peer-read each other's generated scripts against the plain-English sentences → one shared server session where each student runs `/sk reload <their file>` and their own give command in front of the class. That makes the server a demo day rather than a dependency, which matters when 25 students would otherwise be taking turns.

Each student needs their own filename **and their own give command** — the command field exists because two students with `/getweaponset` on one server collide. Variables are keyed by item name plus UUID, so those only collide if two students pick the same item name.

Student work never leaves the browser. Nothing is uploaded, and the hosted page collects nothing.

## Hosting it

The tool is one self-contained HTML file with no external requests. Name it `index.html` and serve it from anywhere static:

- **GitHub Pages** — commit as `index.html`, then Settings → Pages → source `main` / root.
- **LMS upload** (Canvas, Schoology, Classroom) — the most reliable route for students; works with no network.
- **Google Drive** — share view-only; students may need to download it rather than preview it.

It also runs fine opened directly from a downloaded file.

## Caveats

- The generated Skript is built from documented 2.9 syntax but has not been regression-tested against every version and fork. A few of the newer lines are worth smoke-testing on your own server before students rely on them: `on right click on living entity:`, `create a fake explosion`, and the `play sound` syntax.
- The playtest is a teaching simulation, not an emulator. It models triggers, charges, cooldowns, target lookups and a single mob — not real damage formulas, armour values, or tick timing.
- Downloads (`Download .sk`, `Hand in design`) can be blocked by managed Chromebook profiles. The copy buttons are the fallback.
- Four items is a fixed ceiling, matching the assignment.

## Credits

Classroom material for a Skript unit. Generated code patterns follow a working `Thornwood Regalia` reference script.
