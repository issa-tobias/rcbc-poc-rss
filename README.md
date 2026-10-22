# RCBC POC — RSS feed

A filtered Facebook corpus for the RCBC proof of concept, published as RSS 2.0 for
ingestion by Meltwater.

**Feed URL:** https://issa-tobias.github.io/rcbc-poc-rss/feed.xml

## Contents

| | |
|---|---|
| items | 788 |
| platform | Facebook (posts) |
| date range | 2018-11-01 → 2026-10-20 |
| languages | Tagalog / Taglish and English |
| scope | the RCBC conversation — the six accounts named in the POC brief (RCBC PULZ GROUP, RCBC Hexagon Club • RCBC Credit Cards (Bankard), RCBC COMPLAINTS GROUP, RCBC ATM GO, PH Digital Banks, KasKasan Buddies), the wider RCBC ecosystem (the DiskarTech group, the RCBC / RCBC Credit / DiskarTech pages, SSS and card communities), and RCBC-mentioning posts found in neighbouring PH banking groups |
| every item has | a working Facebook permalink |

Competitor-group chatter that never mentions RCBC is excluded, as are items with no
permalink.

## Schema

Matches the Messy Network feed schema, as emitted by
`rss.messy.network/feed?requester=…`:

```
channel: title, link, description, docs, generator, language, lastBuildDate
item:    title, link, description, pubDate, source(url=…), [reach], guid, author
```

- `title` repeats `description` verbatim — the full post text, untruncated.
- `guid` is `messy_fb_<facebook_post_id>`, stable across rebuilds.
- `source` carries the permalink as its `url` attribute and `Facebook` as its text.
- `reach` is the post's comment count, omitted when zero (703 of 788 items carry it).
- `country` is omitted, as it is on the upstream Facebook feeds.

## Regenerating

From the MessyNet Keyword Discovery repo:

```bash
python3 scripts/rcbc_corpus_feed.py \
  --slice rcbc-conversation --require-link \
  --schema messynet --feed-label RCBC_POC_FB \
  --feed-name feed.xml --out-dir <this repo>
```

Add `--since YYYY-MM-DD` to limit the window — e.g. `--since 2026-10-23` for the POC
week only. Then commit and push; the feed URL updates with the next Pages build.
