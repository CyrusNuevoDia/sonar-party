# Partiful Guest CSV Output Schema

Write exactly one CSV unless the operator explicitly asks for another artifact:

```text
workspace/{yyyy-mm-dd}-{slugified-title}/partiful-guests.csv
```

## Columns

Use exactly these headers, in this order:

```csv
name,email,phone,linkedin,twitter,other_url
```

## Field Rules

- `name`: canonical display name from the Partiful profile heading, falling back
  to the named guest row.
- `email`: blank unless Partiful visibly exposes an email or the operator
  provided one.
- `phone`: blank unless Partiful visibly exposes a phone number or the operator
  provided one.
- `linkedin`: best public LinkedIn profile URL, preferably
  `https://linkedin.com/in/...`.
- `twitter`: public X/Twitter profile URL from the guest profile or bio.
- `other_url`: useful non-LinkedIn/non-Twitter URLs such as an explicitly
  exposed Partiful profile URL, Instagram, YouTube, GitHub, or a personal site.

Separate multiple `other_url` values with:

```text
 |
```

Normalize `x.com` and `twitter.com` profile links consistently in `twitter`.
Strip tracking parameters and trailing whitespace where safe. Preserve a URL's
meaningful path and query parameters when they identify the profile or page.

Do not include:

- RSVP status or taxonomy columns,
- unnamed `and N more` companion placeholders,
- Partiful event URLs,
- `/u/...` links found only under another profile's `Shared Mutuals`,
- CDN images, analytics URLs, or static asset URLs,
- duplicate copies of the same normalized URL.

## QA Checklist

After writing the file, run:

```sh
qsv headers <csv>
qsv count <csv>
```

Confirm:

- headers match the six-column contract exactly,
- CSV row count matches the extracted named rows across all processed RSVP
  tabs,
- tab totals, named rows, and unnamed companion counts are reported separately,
- LinkedIn coverage is reported as a count rather than treated as mandatory,
- blank email and phone fields remain blank rather than guessed,
- `other_url` contains guest-owned public links, not mutual, event, or asset
  URLs.
