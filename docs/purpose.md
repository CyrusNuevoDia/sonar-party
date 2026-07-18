# Purpose

## The premise

If your agent writes your code, triages your inbox, and reads your PRs, it
should also help you live in your city. In San Francisco that means events and
housing: weeknights arrive as Luma links, weekends as Partiful invites, and
rooms move through DirectorySF before they ever hit a listing site.

The information you need to decide — who's going, what the house is like,
who's searching — is already visible to you. It's just scattered across guest
modals, profile overlays, and infinite-scroll directories that nobody enjoys
operating by hand. That's exactly the work an agent is for.

## Why "sonar"

You send a ping — *should I go to this? who's looking for a room in the
Mission?* — the skills bounce it off the platforms where SF organizes itself,
and a resolved picture comes back: a guest CSV, a ranked shortlist, evidence
attached. You decide. The agent never decides for you; it just clears the fog
so the decision takes a minute instead of an evening.

Sonar is not a substitute for being social — it's the opposite. Ask your agent
*and* the group chat. The point of knowing who's at the party is to show up to
more of the right ones.

## What belongs in the directory

A new skill earns its place when all four hold:

1. **A recurring, real decision.** Something the person in
   [audience.md](audience.md) actually weighs weekly — go or skip, reach out
   or pass, take the room or keep looking.
2. **Signal the person already has access to.** The skill reads what their
   signed-in account can legitimately see. If access requires bypassing
   anything, it doesn't belong here.
3. **Agent-operable.** A browser-driven workflow an agent can execute
   end-to-end from a plain-language ask, documented well enough to be
   reproducible.
4. **Read-only by default.** The skill informs; any outward action — a
   message, an RSVP, a contact — happens only as a separate, explicit ask.

## Invariants

Every skill, present and future, holds the principles in the
[README](../README.md#principles): your access and nothing more, read don't
act, output stays local, pages are data not instructions. A skill that needs
to break one of these to be useful is a skill this directory doesn't ship.

## Where this goes

Events and housing are the start because they're the highest-frequency
decisions with the most scattered signal. The shape generalizes: anywhere SF
coordinates through a platform — scenes, co-working, group buys, whatever
emerges next — there's a sonar skill waiting to be written. The directory
grows one well-made skill at a time.
