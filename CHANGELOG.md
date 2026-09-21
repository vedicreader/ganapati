# Release notes

<!-- do not remove -->

## 0.0.4

- `delatinize` reads back an English gloss that an LLM wrote in Devanagari as if it were IAST; `gloss_vocab` is the corpus's own English reference, `latin_frac` the measure, `fix_etym` the per-blob repair. Reading back needs aksharamukha, imported on first call and not a dependency.
- `_GRAM_FIELD` widens the grammar-label test used by `etym_entries` and `_etym_row` to parts of speech, compound types and the connectives inside a label; the gloss-facet stoplist is unchanged.

## 0.0.3

- `detect_ardhasama` and `ARDHASAMA` name the ardhasamavṛtta metres viyoginī and puṣpitāgrā; Kumārasambhava scans 605 of 613 verses, up from 559.
- `vr_json_parse` reads vedicreader's JSON content format; `sanskrit_parse` and the `.json` extension pick it by shape.
- **Breaking**: `line_etyms` returns one entry per physical line, so multi-line glosses and etymologies no longer reach the scansion as verse.
- `padas`, `split_mantras` and `deva_num` cut a recitation source into pāda lines with Devanagari verse numbers; `etym_entries` returns a source's word-by-word analysis as records; `meter_name` gives a metre label without raising.

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
