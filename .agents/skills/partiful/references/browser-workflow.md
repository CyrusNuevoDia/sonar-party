# Partiful Browser Extraction Workflow

Use this procedure when the operator is logged in to Partiful or has opened the
event in the in-app browser.

## Read Event Metadata

Start from the current Partiful event URL, normally `/e/<event-id>`. Read the
visible `h1` title and the event's `<time datetime>` value before opening the
guest list. Use the calendar-date portion of `datetime` for the output directory.
If its time-of-day conflicts with the visible time, preserve the date but report
the visible event time; do not silently reinterpret the event.

Normalize the date to `yyyy-mm-dd`. Slugify the title by lowercasing, removing
emoji and punctuation, converting ampersands to `and`, dropping empty words,
and joining remaining ASCII words with hyphens.

## Open The Guest Dialog

Find the `Guest List` section and click its unique `View all` button. Confirm the
dialog heading is `Guest List` and read every visible RSVP tab's label and count,
for example:

```text
Going 40
Maybe 11
```

Process every visible tab. Do not infer absent groups such as invited, declined,
or waitlisted guests.

## Load Named Rows

Partiful can render a full selected tab immediately, but larger lists may require
scrolling inside the guest dialog. Scroll the dialog's list container until the
set of visible named rows stops growing. Do not scroll `document.body`.

Guest rows usually contain a display name, optional bio text, and either an
avatar whose `alt` is the name or initials without an image. Read the visible
row name rather than relying only on image `alt` values.

Text such as `and 1 more` represents unnamed companions attached to a named
guest. Add those numbers to the tab's unnamed-companion total, but do not emit
placeholder CSV rows. The tab's advertised count can therefore exceed the named
row count.

## Open Guest Profiles

Use the latest browser snapshot and scope interactions to the guest dialog.
Click a unique avatar or exact visible guest name. The profile opens as a second,
topmost dialog over the guest list.

Read:

- the profile heading as the canonical display name,
- the visible bio,
- external links in the profile overlay.

Partiful profile overlays can expose personal websites, Instagram, X/Twitter,
and other public URLs. A URL written in the bio may be rendered as an external
link. Classify LinkedIn and X/Twitter URLs into their dedicated CSV columns and
put remaining useful public URLs in `other_url`.

The overlay does not reliably expose the subject's own `/u/<user-id>` URL. Links
under `Shared Mutuals` belong to other people, and links under `Shared Events`
belong to events. Never attribute either to the current guest. Include a
Partiful profile URL only when the UI explicitly exposes the subject's own URL
or navigation changes to that URL.

Close the top profile dialog with its cross control and wait for it to disappear
before opening the next guest. Keep the underlying guest dialog open and retain
the selected RSVP tab.

## Extraction Shape

Use this shape internally before writing CSV:

```ts
type PartifulGuestRow = {
  name: string;
  email: "";
  phone: "";
  linkedin: string;
  twitter: string;
  other_url: string;
};
```

Preserve distinct named roster rows. Do not deduplicate solely by display name;
different guests can share a name. Remove only rows proven to be the same profile
or exact extraction duplicates.
