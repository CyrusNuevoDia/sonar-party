# Luma Browser Extraction Workflow

Use this procedure when the operator is logged in to Luma or asks to use the
in-app browser session.

## Open The Event

Start from the current Luma tab when the operator has already opened or logged in.
If starting from Luma home, locate the target event by visible title and host. Use
the event page URL once known.

For browser work, load `browser:control-in-app-browser` first and use its in-app
browser API. Prefer DOM snapshots and targeted locators over broad visual guessing.

## Read Event Metadata

Read the event title and date from the Luma event page before opening the guest
dialog. Prefer structured metadata from the page when present, such as JSON-LD,
Open Graph tags, or Luma's embedded page data. Fall back to visible page text only
when structured metadata is absent.

Normalize the event date to `yyyy-mm-dd` in the event's local timezone when Luma
exposes one. If the event spans multiple dates, use the start date. Slugify the
title by lowercasing, removing emoji and punctuation, converting ampersands to
`and`, dropping empty words, and joining remaining ASCII words with hyphens.

Create the output directory before writing the CSV:

```text
workspace/{yyyy-mm-dd}-{slugified-title}/
```

## Open The Guest Dialog

On the event page, find the visible guest button. In current Luma pages this often
has a class like `guests-button` and text like:

```text
Aaron Francis, Demir Eren and 64 others
```

There may be duplicate desktop/mobile copies in the DOM. Scope to the visible
button before clicking.

After the dialog opens, confirm the dialog heading, for example:

```text
66 Guests
```

If Luma shows a note that guests without completed profiles are hidden, the
extractable row count may be lower than the heading. Record the visible profile
count in the final summary rather than creating placeholder rows.

## Scroll The Dialog, Not The Page

Luma may render the guest list inside a modal/dialog scroll container. Scroll that
container rather than `document.body`. If the visible DOM already contains the full
count, extraction can proceed. Otherwise, use the browser interaction API to scroll
the visible dialog area and re-check the rendered user links.

Avoid mutating page state in read-only `evaluate` calls. If direct DOM assignment
fails, use the browser interaction/DOM interaction scroll API.

## Extract Rows From The Rendered Dialog

Extract only attendee links from the dialog area. User profile links usually match:

```text
/user/<handle-or-id>
https://luma.com/user/<handle-or-id>
```

Names can be read from the profile image alt text:

```text
Profile picture for Aaron Francis
```

or from the link text. Luma accessible text may duplicate names (`Name Name`), so
collapse exact repeated halves.

Nearby sibling links often include X/Twitter and Instagram links. Capture them for
the row before the next Luma profile link starts.

## Fetch Luma Profile Pages For LinkedIn

The event dialog often omits LinkedIn, but individual Luma profile pages can expose
it in page HTML. Fetch each attendee's Luma profile URL and search links in the
HTML for:

```text
linkedin.com/in/
linkedin.com/pub/
linkedin.com/company/
```

Also merge profile-page X/Twitter, Instagram, YouTube, GitHub, and personal-site
links when present. Do not include Luma CDN image URLs, analytics URLs, or generic
Luma/navigation URLs as personal websites.

Keep fetches bounded and polite. For a room of roughly 50-200 people, limited
parallelism such as 4-8 concurrent profile fetches is enough.

## Useful Extraction Shape

Use this row shape internally before converting to CSV:

```ts
type LumaGuestRow = {
  name: string;
  email: "";
  phone: "";
  linkedin: string;
  twitter: string;
  other_url: string;
};
```

`other_url` should include the Luma profile URL and any non-LinkedIn/non-Twitter
social URLs worth preserving, separated by ` | `.

## Minimal Browser-Side Extraction Pattern

This snippet documents the shape of the extraction. Adapt selectors to the current
Luma DOM and the browser-control API available in the session.

```js
function clean(text) {
  return (text || "").replace(/\s+/g, " ").trim();
}

const heading = Array.from(
  document.querySelectorAll("h1,h2,h3,[role='heading']"),
).find((el) => /^\d+ Guests$/.test(clean(el.textContent)));

let root = heading;
for (let i = 0; i < 10 && root?.parentElement; i += 1) {
  root = root.parentElement;
  const count = root.querySelectorAll(
    "a[href^='/user/'], a[href^='https://luma.com/user/']",
  ).length;
  if (count >= 20) break;
}

const profileLinks = Array.from(
  (root || document).querySelectorAll(
    "a[href^='/user/'], a[href^='https://luma.com/user/']",
  ),
);
```

The final implementation should dedupe by Luma profile URL and then enrich each
row from that profile page.
