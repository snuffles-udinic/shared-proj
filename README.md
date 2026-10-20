# shared-proj

Small static web apps to share with family. Each folder is one app served by GitHub Pages. No accounts, no server, nothing to install.

## Checklist — `checklist/`

**https://snuffles-udinic.github.io/shared-proj/checklist/**

A tap-to-check list that lives entirely inside its link. Make a list, send the link, and whoever opens it gets big checkboxes, a progress bar, an "All done!" banner and picture thumbnails (tap one to see it full screen). Checks are saved on that phone, so closing the tab doesn't lose them. Good for library books to return, packing lists, chores, groceries, a party shopping run.

### Make one yourself

1. Open the link above (with nothing after it). That's the builder.
2. Give the list a name and type one item per line. After a `|` you can add a note and/or a picture URL:
   ```
   Dog Man: Big Jim Believes | due Nov 6
   Sunscreen | for the beach | https://example.com/sunscreen.jpg
   Towels
   ```
3. For books, tap **Find book covers** (from Open Library), and **Try another cover** if one is wrong.
4. Tap **Create link**, then Copy or Share it. That link *is* the checklist.

To change a list later, open it and tap **Make a copy / edit**. You get a new link; the old one keeps its old contents.

### Or ask an AI to make it

Point an assistant (Claude, ChatGPT, …) at this README and give it your items, for example:

> "Using https://github.com/snuffles-udinic/shared-proj, make me a checklist link called 'Camping trip' with: tent, sleeping bags, flashlight, marshmallows. Find a picture for each."

> "Here's a photo of my library receipt. Make a checklist of the books with their covers, and put who borrowed each one and the due date as the note."

The assistant builds the link itself using the format below and sends it back to you.

### Link format (for the AI)

The page reads everything after `#` as base64url (UTF-8, `=` padding removed) of compact JSON:

```json
{"t": "List title", "i": [{"n": "Item name", "s": "optional note", "c": 12345678, "img": "https://optional/image.jpg"}]}
```

- `c` is an Open Library cover ID (`cover_i` from `https://openlibrary.org/search.json?q=...`); the page shows `https://covers.openlibrary.org/b/id/{c}-M.jpg`.
- `img` is any direct image URL; use it for non-book items.
- Python: `BASE + "#" + base64.urlsafe_b64encode(json.dumps(obj, separators=(",", ":"), ensure_ascii=False).encode()).decode().rstrip("=")`

**Keep in mind:** the list lives in the link, not on GitHub, so only people with the link see it. Links are comfortable up to about 50 items (very long ones can break in SMS or email). Checks don't sync between phones; each phone keeps its own.
