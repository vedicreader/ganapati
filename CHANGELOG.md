# Release notes

<!-- do not remove -->

## 0.0.3

The vedicreader JSON content format, and an etymology split that survives Devanagari.

- `vr_json_parse` reads vedicreader's JSON content format; `sanskrit_parse` picks it by shape, and
  `.json` joins the `sanskrit_verse` profile's extensions behind a reader-specific sniff.
- `vr_xml_parse` reads section-level `etymology`, which is where vedicreader keeps it. Previously
  only a line-level attribute was read, and no library file has one.
- `_etym_terms` splits an etymology entry on separators. A `\w` token class turned `धर्मक्षेत्रे`
  into `धर` and `मक`; an ASCII one turned `agnā` into `agn`. Both fragments are gone from the
  `lemma` and `gloss` facets.

## 0.0.2

Partial verses, audio timings, and the analysis a source already carries.

- `match_pada` names the metres one pāda fits; `group_verses` finds how many lines make a verse
  when forced alignment or ASR has left no daṇḍa to split on.
- `detect_meter` has one not-found state: `None` only when nothing scanned, otherwise an
  `AttrDict` whose `name` is None. It no longer returns `None` for a verse that is not
  4-divisible. **Breaking** for callers testing the result for truthiness.
- `vr_xml_parse` skips `ignore="true"` lines, which are printed but not recited, and keeps
  `start_time_ms`, `end_time_ms`, `alignment_id` and `etymology`.
- `mora_rate`, `timing_check` and `guru_laghu_ratio` measure a scansion against its own audio.
  `verse_meta` gains a `mora_rate` facet and a `timing` facet that flags the disagreement.
- `etym_facets` reads a source's own analysis; `source_meta` is metre plus timings plus that, and
  is what `sanskrit_meta(None)` now returns. vidyut is called only for chunks that lack one.
- `mora_weights` returns `(syllable, 2|1)` beside `syllables`' `(syllable, guru)`.

## 0.0.1
ganapati release to parse sanskrit texts
