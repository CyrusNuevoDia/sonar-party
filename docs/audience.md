# Audience

## Is this you?

You live in San Francisco, or close enough that your calendar doesn't know the
difference. Your week fills up through links: a Luma event someone dropped in
Discord, a Partiful invite from a housemate's cofounder, a DirectorySF profile
a friend forwarded with "you two should live together."

You keep a terminal open with Claude Code or Codex in it, and you've long
since stopped limiting your agent to code. You'd rather phrase a question in
plain language than click through an infinite-scroll modal. Before committing
a Tuesday night you want to know who's actually in the room — and when you
find out, you're more likely to go, not less.

If that's you, install the plugin and ask your agent who's at the party.

## The persona, precisely

This section is the contract for anyone — human or agent — writing a new
skill for the directory. Build for this person and nobody else.

**Who they are.** An AI-native San Franciscan: an engineer, founder,
researcher, or builder who treats their agent as a daily collaborator. They
are fluent enough to install a plugin and read a SKILL.md, and busy enough
that they never will if the skill demands babysitting.

**How they operate.** They ask in one sentence and expect the agent to run
the browser, do the scrolling, and come back with an artifact — a CSV, a
ranked shortlist — plus an honest account of coverage ("42 of 66 guests had
visible profiles"). They spot-check the artifact; they don't supervise the
process.

**Their recurring decisions.**

- *Go or skip* — is this event worth the night? Who's going that I know, or
  want to?
- *Prep for the room* — I'm going; who should I make a point of meeting?
- *Housing through the network* — who's searching, what's open, what actually
  fits my dates, budget, and tolerance for housemates?

**What they won't tolerate.**

- Skills that act socially on their behalf — messaging, RSVPing, contacting.
  Their name touches nothing they didn't explicitly send.
- Data leaving their machine. The guest list is for their eyes, not a
  webhook's.
- Fabricated confidence — invented rents, guessed attendance, placeholder
  rows. They'd rather have 40 real rows than 66 padded ones.
- Access games. If their account can't see it, the skill doesn't have it.

**The bar.** A new skill ships when this person would install it: it answers
a decision they face this week, runs from one plain-language ask, respects
every principle in the [README](../README.md#principles), and returns an
artifact they'd actually open. If they'd read the SKILL.md and shrug, it's
not ready.
