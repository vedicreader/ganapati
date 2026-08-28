# Working in this repo

nbdev. The notebooks under `nbs/` are the source; `ganapati/*.py` is generated. Edit the notebook,
run `nbdev_export`, never edit the `.py`. CI runs `nbdev_export` and fails on a diff.

## The line with litesearch

litesearch keeps the fold and the FTS5 tokenizer, because those are on for every store and are
the largest measured retrieval win there. It also keeps `CITE_RE` and `cite_parts`, because
`tree.py` builds the document tree out of a citation.

Everything else about Sanskrit is here. ganapati depends on litesearch, never the reverse: adding
a litesearch import to this repo is fine, adding a ganapati import to litesearch is a cycle.

## Three modules, in dependency order

`text` has no ganapati dependencies. `metre` imports `verse_spans` from `text`. `lemma` imports
from both and holds the litesearch profiles. Keep it that way.

## vidyut is optional and must stay optional

`sanskrit_meta(None)` returns `verse_meta` itself, the same object, so the metre-only path costs
nothing. Every vidyut and Monier-Williams import is inside the function that needs it and raises
saying what to install. There are no extras.

## Prose in notebooks

Short. Lead with what the code does. Numbers instead of adjectives. No em dashes, no bold inside
a paragraph, no rhetorical questions. A rationale longer than three sentences belongs in a
docstring.

## Docstrings and comments

One line. A second sentence only for a measured number or a footgun. Inline comments in a `def`
signature are nbdev docments and become the API parameter table.

## Test with real verses

The metre assertions use Meghadūta 1.1 and Gītā 1.1, not invented strings. A metre detector that
passes on made-up syllables has not been tested.
