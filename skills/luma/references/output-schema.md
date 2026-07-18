# Luma Guest CSV Output Schema

Write exactly one CSV unless the operator explicitly asks for additional artifacts.
The file should be named for the source, usually:

```text
workspace/{yyyy-mm-dd}-{slugified-title}/luma-guests.csv
```

Derive `{yyyy-mm-dd}` and `{slugified-title}` from the Luma event page. Create
the event directory under `workspace/` when missing.

## Columns

Use exactly these headers, in this order:

```csv
name,email,phone,linkedin,twitter,other_url
```

## Field Rules

- `name`: attendee display name from the Luma guest dialog or profile page.
- `email`: blank unless Luma visibly exposes an email or the operator provided one.
- `phone`: blank unless Luma visibly exposes a phone number or the operator provided
  one.
- `linkedin`: best public LinkedIn URL, preferably `https://linkedin.com/in/...`.
- `twitter`: public X/Twitter URL from the dialog or profile page.
- `other_url`: preserve useful non-LinkedIn/non-Twitter URLs such as:
  - the attendee's Luma profile URL,
  - Instagram,
  - YouTube,
  - GitHub,
  - a personal/company website.

Separate multiple `other_url` values with:

```text
 |
```

Do not include:
- `category`
- `category_2`
- `category_3`
- `intent_ask`
- `intent_ask_2`
- `context`
- `context_2`
- `context_3`
- `profileFetchStatus`
- `profileTitle`
- Luma CDN images
- analytics or static asset URLs

## QA Checklist

After writing the file, run:

```sh
qsv headers <csv>
qsv count <csv>
```

Confirm:
- headers match the six-column contract exactly,
- row count matches the visible Luma profile rows; when Luma hides incomplete
  profiles, report the dialog count and visible row count separately,
- LinkedIn coverage is reported as a count, not treated as mandatory for every
  row,
- blank email/phone fields remain blank rather than guessed,
- `other_url` contains Luma/social/profile URLs, not asset URLs.
