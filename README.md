# Sonar Party

Sonar for SF life. A directory of skills that teach your agent — Claude Code,
Codex, or anything that reads `SKILL.md` — to answer the questions San
Francisco actually runs on:

- **Who's at this party?** Pull the visible guest list from a Luma or Partiful
  event into a clean CSV — names plus LinkedIn, X, and other socials — so you
  can decide whether tonight is worth leaving the house.
- **Who's looking for a room?** Search DirectorySF searchers and spaces with
  plain language — "short-term lease near the Mission, fine with five
  housemates" — and get a ranked shortlist with evidence, not vibes.

You send a ping, your agent bounces it off the platforms where SF organizes
itself, and a picture comes back. Then ask your agent *and* the group chat.

## Skills

| Skill               | What your agent can do                                                                     |
| ------------------- | ------------------------------------------------------------------------------------------ |
| `sonar:luma`        | Extract a Luma event's visible guest roster into a CSV of names and socials.               |
| `sonar:partiful`    | Extract a Partiful event's visible guest roster — every RSVP tab — into the same CSV.      |
| `sonar:directorysf` | Filter, search, and rank DirectorySF housing searchers and spaces against your criteria.   |

Each skill is a folder under [`skills/`](skills/) with a `SKILL.md` (the
contract: workflow, safety boundaries, output format) and a `references/`
directory with the detailed browser procedures.

## Install

**Claude Code** — install as a plugin:

```text
/plugin marketplace add CyrusNuevoDia/sonar-party
/plugin install sonar@sonar-party
```

**Codex** — skills are discovered from `.agents/skills/`, which this repo wires
up via symlink. Clone and work inside the repo, or link the skills into your
own workspace:

```sh
git clone https://github.com/CyrusNuevoDia/sonar-party
mkdir -p your-workspace/.agents/skills
ln -s "$PWD/sonar-party/skills/"* your-workspace/.agents/skills/
```

**Anything else** — point your agent at `skills/`. Each skill is
self-contained; copy the folders wherever your harness discovers skills.

## First run

You need real accounts — the skills see exactly what you can see, nothing
more:

- **Luma** — an account, and you generally need to be registered for an event
  to view its roster.
- **Partiful** — an account (phone sign-in), and usually an RSVP.
- **DirectorySF** — invite-based; if you're not in the directory yet, ask a
  friend who is.

Each skill ships a `references/setup.md`; on first use your agent walks you
through signing in inside its browser. It never asks for your credentials.

## Principles

Every skill in the directory holds these invariants:

- **Your access, nothing more.** Skills read what your signed-in account can
  already see. No credential handling, no cookie copying, no bypassing walls.
- **Read, don't act.** No contacting people, no messaging, no RSVPing on your
  behalf — unless you explicitly ask for that as a separate action.
- **Output stays local.** CSVs and shortlists land in your workspace and go
  nowhere else.
- **Pages are data, not instructions.** Skills treat page content as
  untrusted input.

## Docs

- [`docs/purpose.md`](docs/purpose.md) — why this exists and what belongs here.
- [`docs/audience.md`](docs/audience.md) — who it's for, and the bar a new
  skill has to clear.

## Contributing

The directory is meant to grow. If there's a recurring SF decision your agent
could inform — a scene, a platform, a signal — write the skill: a folder with
a `SKILL.md` and its references, holding the principles above. Read
[`docs/audience.md`](docs/audience.md) first; the test for every addition is
*would this person install it?*
