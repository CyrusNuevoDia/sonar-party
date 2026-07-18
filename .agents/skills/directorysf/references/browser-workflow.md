# DirectorySF browser workflow

Use the logged-in in-app browser. Direct unauthenticated GET requests to the tRPC routes
return `401`, so do not fall back to shell HTTP clients or inspect cookies/session storage.

## Observe and operate

1. Navigate to `/searchers` or `/spaces` and wait for the status
   `Loading directory listings` to disappear.
2. Take a DOM snapshot before each filter interaction. Confirm each locator resolves to
   exactly one element before acting.
3. Apply filters by their visible labels. The page URL stays unchanged.
4. After changing a filter, wait for the loading status to disappear before reading cards.
5. Scroll the page to the bottom and recount unique primary URLs. Continue while the count
   increases.

## Observed request contracts

Searchers use:

```text
GET /api/trpc/profiles.getHousingSearchProfiles
```

The decoded input has this shape:

```js
{
  limit: 25,
  filters: { leaseLength?, housemateCount?, movingTime? },
  cursor: 25, // omitted on the first page; then 25, 50, 75, ...
  direction: "forward"
}
```

Observed UI-to-payload examples were `1-year Lease -> leaseLength: "1"`,
`3-5 housemates -> housemateCount: "2"`, and `ASAP -> movingTime: "1"`.
Drive the UI by labels instead of assuming unobserved enum values.

Spaces use:

```text
GET /api/trpc/spaces.getCommunities
```

The decoded input has this shape:

```js
{
  limit: 10,
  filters: {
    roomPriceRange: null,
    availableByDate: null,
    showCommercial: null
  },
  cursor: 10, // omitted on the first page; then 10, 20, 30, ...
  direction: "forward"
}
```

Observed examples were `Under $2,000 -> roomPriceRange: "3"`, an availability date as
`YYYY-MM-DD`, and the checked commercial toggle as `showCommercial: true`.

When request verification matters, use the tab's observed page-asset inventory and select
resources whose names are `profiles.getHousingSearchProfiles` or
`spaces.getCommunities`. Do not include unrelated RSC prefetches or analytics requests.

## Extract loaded cards

The result cards currently use a container containing these stable signals:

- one or more `/u/...` links and exactly one `Contact me` button for Searchers;
- one repeated primary `/s/...` link, an owner `/u/...` link, and one `Contact me` button
  for Spaces;
- the card container has the classes `bg-surface-alt` and `rounded-2xl`.

Use one bounded page evaluation after loading is complete:

```js
const clean = (value) => (value || "").replace(/\s+/g, " ").trim();
const cards = Array.from(
  document.querySelectorAll("div.bg-surface-alt.rounded-2xl"),
);

return cards.map((card) => {
  const space = card.querySelector("a[href^='/s/']");
  const users = Array.from(card.querySelectorAll("a[href^='/u/']"));
  const primary = space || users[0];
  const links = Array.from(card.querySelectorAll("a[href]"))
    .map((link) => new URL(link.getAttribute("href"), location.origin).href)
    .filter((url, index, all) => all.indexOf(url) === index);

  return {
    kind: space ? "space" : "searcher",
    title: clean(primary?.textContent),
    url: primary ? new URL(primary.getAttribute("href"), location.origin).href : "",
    owner: space ? clean(users[0]?.textContent) : "",
    text: clean(card.innerText),
    links,
  };
});
```

Deduplicate by `url`. Keep card text for matching, but suppress phone numbers and email
addresses in the final response unless the user explicitly asks for contact details.
