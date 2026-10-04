# corpus-cisa-kev

A plain-text mirror of the **CISA Catalog of Known Exploited Vulnerabilities
(KEV)**, one file per CVE, for use as a retrieval corpus.

- **Source feed:** https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json
- **Catalog page:** https://www.cisa.gov/known-exploited-vulnerabilities-catalog
- **Fetched:** 2026-10-04 (feed self-reports `catalogVersion: 2026.10.04`, `dateReleased: 2026-10-04T18:52:56.0635Z`)
- **Entries:** 1734 (feed `count` field matches the number of `vulnerabilities` elements and the number of `kev/*.txt` files)
- **dateAdded range in this snapshot:** 2021-11-03 to 2026-10-04
- **knownRansomwareCampaignUse in this snapshot:** 361 `Known`, 1373 `Unknown`

## Layout

- `kev/CVE-YYYY-NNNNN.txt` — one labelled plain-text file per KEV entry (1734 files)
- `source/known_exploited_vulnerabilities.json` — the exact feed the text files were generated from, kept for verification
- `MANIFEST.tsv` — TSV mapping every path to the URL it was derived from

## File format

Each file is UTF-8, one `label: value` pair per line, in this order:

```
catalog: CISA Catalog of Known Exploited Vulnerabilities
catalogVersion: 2026.10.04
dateReleased: 2026-10-04T18:52:56.0635Z
cve: CVE-YYYY-NNNNN
vendorProject: ...
product: ...
vulnerabilityName: ...
shortDescription: ...
dateAdded: YYYY-MM-DD
dueDate: YYYY-MM-DD
knownRansomwareCampaignUse: Known|Unknown
forensicTriage: Yes|No
cwes: CWE-119, CWE-...        (omitted when the feed's cwes list is empty — 175 entries)
requiredAction: ...
notes: ...
```

All labels correspond 1:1 to fields in the source JSON (`cve` = the feed's
`cveID`). Values are verbatim from the feed with internal whitespace collapsed
to single spaces (the feed contains no embedded newlines — verified across all
1734 entries). `requiredAction` is a **per-entry** field in this feed version
(present on all 1734 entries), not a single catalog-wide value.

## Licence

**CC0 1.0 Universal (public domain dedication).** Verbatim from the license
file CISA links from the catalog page
(https://www.cisa.gov/sites/default/files/licenses/kev/license.txt):

> "The KEV database is distributed under the Creative Commons 0 1.0 License."

The file then reproduces the full CC0 1.0 Universal legal code. As a work of
the US federal government (CISA/DHS), the catalog is additionally not subject
to copyright in the US (17 USC 105). Nothing on the catalog page or in the
feed contradicts public-domain/CC0 status; the license file notes that third-
party links remain under their own policies and that use of the data does not
authorize use of the CISA logo or DHS seal.

## Regeneration

```
curl -o kev.json https://www.cisa.gov/sites/default/files/feeds/known_exploited_vulnerabilities.json
# then expand each vulnerabilities[] element into kev/<cveID>.txt as described above
```

This mirror is a snapshot; the upstream catalog is updated continuously.
