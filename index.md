# Where these packs come from

Ten gzipped NDJSON files, about 2.17 million cards, 119 MB. Each line is one
recording: who played on it, on what, and where it was issued.

Licensing is in [LICENSE](LICENSE) — the short version is that the facts are
CC0 and free, and the compilation around them is CC BY-SA 4.0.

## The source

**Everything in these files is derived from one source: the Discogs monthly
data dump of 1 August 2026** (`discogs_20260801_releases.xml.gz` and
`discogs_20260801_artists.xml.gz`), published at <https://data.discogs.com/>
under CC0.

Discogs is a community-built database. The credits here were entered by
thousands of contributors reading the backs of records, and the whole thing
rests on that work.

## What each field is

| field | what it holds | where it comes from |
|---|---|---|
| `k` | the card key: artist and tune, normalised | derived |
| `a` `t` `al` | artist, tune title, release title | Discogs release and track |
| `p` | performers, as `[name, instrument]` | Discogs extraartist credits |
| `w` | writing credits, kept separate from performers | Discogs |
| `pr` | production and engineering credits | Discogs |
| `rid` | the Discogs release id we chose | Discogs |
| `lb` | the label | Discogs |
| `rel` | the year of issue | Discogs release year |
| `rec` `rp` | the recording date and how precise it is | Discogs recording-date field |
| `ch` | mono or stereo | Discogs |
| `lv` | whether it is a live recording | Discogs |
| `ti` | how firmly the personnel attach to this track | derived from credit scope |

`ti` and `k` are ours: they are judgements about the Discogs data rather than
values read out of it. Everything else is Discogs, reshaped.

## What is deliberately not here

The app shows things these files do not contain, and they are absent on
purpose — either because their licences differ from CC0, or because we do not
yet have permission to redistribute them:

- **Recording places and studios**, and album recording spans, which come from
  Wikipedia infoboxes. Wikipedia is CC BY-SA, which is compatible, but those
  fields ship inside the application rather than in these packs.
- **Session logs** from jazzdisco.org and ellingtonia.com, which supply exact
  session days and full personnel for a few thousand cards. These are not
  CC0 and are not redistributed here.
- **Biographical dates** (born, died, birthplace, country) from Wikidata,
  which is CC0, and identifiers from the Discography of American Historical
  Recordings. The DAHR material is held provisionally, pending permission, and
  is not redistributed here.

If a card in the app carries a studio, a place, or a "date read off the
tracklist", that did not come from these files.

## How they were built

`ingest/parse_discogs.py` turns the XML dump into `credits.sqlite`;
`ingest/build_canon.py` and `ingest/build_all_packs.py` choose one pressing per
recording across every pack at once and write the NDJSON. The pressing is
chosen globally rather than per pack, so two packs that both carry a tune carry
the identical card.

## The packs

Built 2026-08-24. `dated` is how many cards carry a recording date rather than
only a year of issue; it is stated because a pack named for an era is partly
made of cards placed by their pressing.

| pack | cards | dated | size |
|---|---:|---:|---:|
| Early jazz and swing | 231,616 | 28,130 | 13.5 MB |
| Bebop to the sixties | 461,066 | 91,431 | 24.6 MB |
| Loft, ECM and after | 709,287 | 158,988 | 39.6 MB |
| Country blues | 1,951 | 1,404 | 0.1 MB |
| Chicago and the electric blues | 11,814 | 6,840 | 0.7 MB |
| Rhythm and blues | 7,478 | 1,591 | 0.5 MB |
| Gospel | 17,611 | 2,168 | 0.9 MB |
| Cuban and Latin | 535,025 | 29,703 | 27.8 MB |
| Brazilian | 119,584 | 16,799 | 7.2 MB |
| African | 78,480 | 4,522 | 4.3 MB |

Packs overlap by design — 18.7% of card slots — because a pack is a subject
rather than a partition. The app de-duplicates on merge.

## A caution about the dates

Roughly 4% of dated cards carry a date that contradicts something else on the
card: a recording date later than the release, a musician who had already died,
or an album whose own title names different years. `ingest/check_dates.py
--impossible` finds them. They are not filtered out of these files, because a
silent filter would make the corpus look better than it is. Treat a date here
as good evidence and not as a citation.

## Contact

<adam@sleevenotes.ink> · <https://sleevenotes.ink>

Corrections are welcome, and corrections to Discogs itself are worth more than
corrections to us: they reach everybody.
