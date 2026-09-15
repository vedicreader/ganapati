# Working in this repo

nbdev. The notebooks under `nbs/` are the source; `ganapati/*.py` is generated. Edit the notebook,
run `nbdev_export`, never edit the `.py`. `README.md` comes from `nbs/index.ipynb` through
`nbdev_readme`. CI runs `nbdev_export` and fails on a diff.

## Dependency direction

ganapati imports litesearch. Never the reverse, in code or in `pyproject.toml`: litesearch
naming ganapati in any dependency group, dev included, is a cycle.

## Three modules, in order

`text` has no ganapati dependencies. `metre` imports `verse_spans` from `text`. `lemma` imports
from both and registers the reader profiles. Keep that order.

## vidyut is optional and must stay optional

`sanskrit_meta(None)` returns `source_meta`, which is metre, audio timings and the source's own
etymology: pure regex and scansion, no vidyut import and no download. Every vidyut and
Monier-Williams import is inside the function that needs it and raises saying what to install.
There are no extras. When a pipeline is supplied, the source's own analysis still wins and vidyut
is called only for the chunks that lack one.

## Test with real verses

The metre assertions use Meghadūta 1.1, Gītā 1.1 and Kumārasambhava 4.1, whose viyoginī is the
ardhasamavṛtta case: odd pādas of 10 syllables, even of 11, so neither the pāda table nor the
mora family can name it. A metre detector that passes on invented
syllables has not been tested. `nbs/mahabharata.htm` is GRETIL's Mahābhārata 1.1, 210 verses: the
end-to-end `Index.add` assertions in `02_lemma` read it, so the test run needs it present.

## detect_meter has one not-found state

`None` means nothing scanned. Anything else is an `AttrDict` whose `name` is None when no metre
matched, so a caller checks `name`, never truthiness. `match_pada` and `group_verses` are what a
partial verse goes through: forced alignment and ASR leave no daṇḍa for `verse_spans` to split on.

## Timings travel as text

A litesearch `meta` callable sees a chunk's text and nothing else, so the vedicreader readers write
each aligned line's span into the line as `[t 0-1800 a1]` and its etymology as a `> etym:` line.
Brackets and `>` are exactly what `metrical_text` strips, so neither reaches the scansion.
`time_marker` writes them, `line_times` and `line_etyms` read them back.

## Two vedicreader readers, one shape

`vr_xml_parse` reads the `<lyrics>` XML, `vr_json_parse` the JSON content format. Both return the
same `(pages, meta)`. In JSON one `role` per line replaces the two display flags, and `verse` is
the only role that is both printed and recited.

## What a vedicreader etymology looks like

Two shapes, one per source. A line's own etymology is `surface, lemma, grammar…, gloss`; a
section's is the LLM pass's, normalised by vedicreader to a bare header line then `- word: gloss`
per word, which `_ETYM_HEAD` reads bullet and all.

Line-level, not section-level in the source XML: 1689 of the corpus's 2619 lines carry one, against one section.
Each entry is `surface, lemma, grammar…, gloss` and a whole word-by-word analysis sits in one
attribute, newline-separated. Two consequences. `_quoted` prefixes every physical line, because
an unprefixed continuation line reaches the scansion as if it were a pāda: that was 8685 lines of
grammar prose being scanned as verse. And `_etym_terms` reads the comma layout, sending surface
and lemma to `lemma`, the gloss tail to `gloss`, and the grammar fields to neither, since the
lemma already implies them. `word: gloss` entries and free prose still split as before, on
separators, because a `\w` token class breaks a Devanagari word at every matra.

## Prose in notebooks

Short. Lead with what the code does. Numbers instead of adjectives. No em dashes, no bold inside
a paragraph, no rhetorical questions. A rationale longer than three sentences belongs in a
docstring.

## Docstrings and comments

One line. A second sentence only for a measured number or a footgun. Inline comments in a `def`
signature are nbdev docments and become the API parameter table.

## evals

`evals/sanskrit_eval.ipynb` is outside `nbs/`, so it is out of the test run, the docs build and
the wheel. It is the measurement behind the encoder table.
