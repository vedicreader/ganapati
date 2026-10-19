# Release notes

<!-- do not remove -->

## 0.1.5

- The `search` extra needs litesearch 0.1.44 or later. The chunker tests check `VerseChunker` and `ProseChunker` against litesearch's public `pack_target` instead of `_pack_target`.

## 0.1.4

- `dhatu` and `made` are public: `dhatu` turns a dhātu field into a vidyut `Dhatu`, and `made` gives the stem vidyut builds from it, alone or with a kṛt (`made('A\px~,svAdi,,,pra', 'kta')` is prApta). `_dhatu` and `_made` still import.

## 0.1.3

- The wheel carries vidyut's licence in its `dist-info/licenses` as well as in `ganapati/data`, through `license-files`; the licence is declared as the SPDX expression `Apache-2.0`.

## 0.1.2

- `join` writes the sandhi between two words given as each stands alone, and names the join with `join_rule`: `join('rAmaH', 'api')` is `('rAmo', "'pi", ('o_a', '-aḥ to -o', '6.1.113'))`, and `join('tava', 'iti')` is `('taveti', None, ...)`, the None marking two words written as one. `samhita` joins a line of words this way and lists the class of each join. A final visarga is read as s, or as r in punar, antar and prātar, and saḥ and eṣaḥ drop it before a consonant (6.1.132).
- The joins come from vidyut's table of 1,468 sandhi rules, now shipped in the package as `ganapati/data/vidyut_sandhi_rules.csv` (`RULES`, read by `rules`). vidyut is MIT licensed; its copyright notice and licence are in `ganapati/data/LICENSE-vidyut`.

## 0.1.1

- New module `ganapati.vyakarana`, derivations through vidyut's prakriya. `pada` and `derive` read a one-string SLP1 spec (`s|rAma|puM|praTamA|eka`, `t|BU,BvAdi,,,|kartari|la~w|praTama|eka|P`) and return the vidyut `Pada` and its `Prakriya`; `derive(spec, surf)` returns only a derivation that reaches the written form. `steps` lists the sūtras applied, each once while the form stays the same. `spec_words`, `spec_lemma` and `spec_root` name a spec's grammar in English, its headword and its root; with the written form, `spec_words` reads a doer-voice verb as middle when only the ātmanepada endings reach it (kurute).
- `stem` gives a feminine stem in -ā or -ī its feminine suffix (`Pratipadika.nyap`), so `s|sItA|strI|caturTI|eka` derives sītāyai, not sītaiḥ.
- `align` places a line's word-by-word entries on its written tokens (स कालेनेह: सः, कालेन + इह), comparing `sandhi_skeleton` forms.
- `join_rule` names the sandhi at a join as `(id, name, sūtra)` from `JOINS`, 25 classes from savarṇa dīrgha (6.1.101) to the visarga of saḥ dropped (6.1.132).
- `dev` is an extra, so CI and the docs build install the notebooks' dependencies.

## 0.1.0

- ganapati no longer depends on litesearch. The Devanagari primitives it read from `litesearch.sanskrit` (`strip_vedic`, `fold_token`, `roman_fold`, `deva2ascii`, `detect_script`, `DEVANAGARI`, `DANDA`, `DDANDA`, `VIRAMA`, `DEVA_DIGITS`, `CITE_RE`, `cite_parts`) live in the new `ganapati.sanskrit`; the runtime needs only fastcore, fastlite and vidyut. A project pinned to an old numpy or tokenizers (vachaka, for Vāgdhenu) can now scan metre with ganapati.
- The Sanskrit profiles no longer register on import. ganapati declares a `litesearch.profiles` entry point and litesearch 0.1.38 calls `register_profiles` the first time it looks a profile up, so `pip install 'ganapati[search]'` gives litesearch the profiles without either package importing the other. Call `register_profiles(nlp=, mw=)` yourself to add lemmas and glosses, as before.

## 0.0.9

- `padas` detaches a speaker tag glued to the pāda with no space again: DharmicData's Gītā writes 11 (`अर्जुन उवाचएवं…`, 12.1; `भगवानुवाचमय्यावेश्य…`, 12.2), and 0.0.8 left them on the pāda because it read any letter after `वाच` as the word going on. Only a vowel sign, virama, nukta, anusvāra, candrabindu or visarga after `वाच`, or a word-final halant (`वाचम्`), keeps the word whole now.

## 0.0.8

- `padas` detaches a `…उवाच` speaker tag only where `वाच` ends the word. GRETIL's Chāndogya 4.2.3 `प्रत्युवाचाह हारेत्वा…` was cut into `प्रत्युवाच` and a line opening on the bare sign `ाह`; a vowel sign, virama, anusvāra, visarga or letter after `वाच` (`वाचं`, `वाचः`, `वाचा`) now keeps the word whole. `धृतराष्ट्र उवाच`, `श्रीभगवानुवाच` and `अर्जुन उवाच ।` are cut as before.
- The scansion reads IAST in NFC: decomposed IAST (`a` + U+0304 for `ā`) lost its long vowels, so Gītā 1.1 scanned 48 morae instead of 50.

## 0.0.7

- New module `ganapati.names`. `name_key` gives one key to the IAST, plain-roman, Indian-English and Devanagari spellings of a name (Bhīṣma, Bheeshma, भीष्म are `bisma`); `name_variants` finds the keys in a pool one vowel longer or shorter (krsna, krishna), never a consonant apart (Bhīma is not Bhīṣma).
- `epithets()` and `canonical()` map 165 epithets of 36 deities and epic figures one way to the names a translation writes: Raṅganātha, Śrīnivāsa, Veṅkaṭeśvara to Viṣṇu, Nārāyaṇa, Hari; Dāśarathi to Rāma; Pārtha to Arjuna; Vaidehī to Sītā; Māruti to Hanumān. A leading Śrī, ṛ typed as `r` (krsna), an English plural and, from five letters, a dropped final `a` (Ganesh, Arjun) are read as the name.
- `term_gloss()` gives short English for 125 śāstra and jyotiṣa terms (anumāna: inference; sthitaprajña: steady wisdom; lagna: ascendant), falling back to a downloaded `mw_lexicon`; it never downloads.

## 0.0.6

- The unclosed verse marker 0.0.5 accepts at a line end is a double daṇḍa only: `॥७`, not `। १`. DharmicData's paryāya sūktas (AV 16.8) write a running count after the marker, `॥४॥ ४। १`, and 0.0.5 read that count as another verse, leaving `४ ॥ १ ॥` sections.

## 0.0.5

- `_VMARK` also accepts a verse marker left open at a line end — `॥७` with no closing daṇḍa, as DharmicData prints Atharvaveda 2.36.7 — so `split_mantras` ends the verse there instead of merging it into the next, and `padas` no longer leaves the bare `७` glued mid-line. A number after a daṇḍa mid-line is still text.

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
