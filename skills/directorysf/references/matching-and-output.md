# Natural-language matching and output

## Interpret the query

Separate the user's request into hard constraints and soft signals.

Hard constraints commonly include:

- Searchers versus Spaces;
- maximum rent or explicit budget band;
- exact or relative move/availability date;
- lease length;
- housemate count;
- required neighborhood or commute;
- whether commercial/for-profit spaces are allowed.

Soft signals commonly include:

- work or research area;
- hobbies and lifestyle;
- founder, student, family, quiet, social, or community preferences;
- desired amenities or house culture;
- fuzzy location preferences such as “near BART” or “close to builders.”

Apply compatible native filters first. Match remaining criteria against the entire loaded
card text, title, owner/name, and external-link domains.

## Rank conservatively

Use a simple evidence hierarchy rather than opaque numerical precision:

- **Strong**: satisfies every visible hard constraint and explicitly mentions multiple
  soft criteria.
- **Good**: satisfies visible hard constraints and explicitly mentions one key soft
  criterion.
- **Possible**: no visible hard conflict, but one or more required facts are missing or the
  semantic evidence is indirect.

Reject visible hard conflicts. Do not treat missing data as satisfied. Use listing recency
only as a tie-breaker unless the user asks for newest results.

For date ranges, require overlap with the user's requested date. “From DATE” is open-ended
unless the description says otherwise. A single displayed date is not proof of a longer
availability period.

For rent, use the displayed price band as the structured fact. A biography or description
may contain a more exact number; mention it only when clearly associated with rent.

## Report results

Lead with the matches, not the browsing process. Use this compact shape:

```markdown
1. [Name or title](DirectorySF URL) — Strong
   - Fits: visible lease/rent/date/location facts
   - Why: direct evidence for the user's free-form criteria
   - Unknown: material missing fact, if any
```

Do not quote long biographies or descriptions. Paraphrase the evidence. Preserve external
links only when they help explain the match.

Finish with:

```text
Inspected N unique listings; loaded X pages; result set exhausted/not exhausted.
```

If pagination was stopped after finding enough matches, say so and offer to sweep the full
directory. If the user asked for all results, continue until a further bottom scroll adds no
new unique primary URLs and report the total.
