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

The metre assertions use Meghadūta 1.1 and Gītā 1.1. A metre detector that passes on invented
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
same `(pages, meta)`. vedicreader keeps `meaning` and `etymology` on the `<section>`, not on the
line, and its etymology is either `word: gloss; word: gloss` or prose: `_vr_etym` takes both and
`_etym_terms` splits on separators, because a `\w` token class breaks a Devanagari word at every
matra. In JSON one `role` per line replaces the two display flags, and `verse` is the only role
that is both printed and recited.

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
