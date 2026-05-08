# Pointe Church App — Content

This repo holds the **weekly-editable content** for The Pointe Church app
([io.pushpay.thepointechurchcocoabeach](https://apps.apple.com/)).

The app fetches `featured.json` (via the Cloudflare Worker proxy at
`pointe-pco-proxy.thepointechurch.workers.dev/content`, which caches it for
60 seconds) on launch and on app foreground. Edits here appear in the app
within ~1 minute of saving.

## How to update content

1. Open `featured.json` on github.com (or in any editor)
2. Tap the pencil icon, edit, commit
3. Wait ~1 minute, force-quit and reopen the app

## Schema

```json
{
  "header": {
    "imageUrl": "https://example.com/hero.jpg or null"
  },
  "sermonNotesUrl": "http://bible.com/events/...",
  "featured": [
    {
      "id": "unique-slug",
      "title": "Card title",
      "dateLabel": "JULY 8–12",
      "description": "Short blurb shown on the card.",
      "imageUrl": "https://example.com/image.jpg or null",
      "url": "https://destination-when-tapped/"
    }
  ]
}
```

- `imageUrl: null` falls back to the bundled default image
- Adding/removing items in the `featured` array is fine — the app handles any length
- `id` should be a stable slug; reusing the same id across edits is OK

## Safety

The Worker validates this JSON before serving it. If you save a syntax error,
the Worker returns the previous good version and the app keeps running.
