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

`sanskrit_meta(None)` returns `verse_meta` itself, the same object, so the metre-only path costs
nothing. Every vidyut and Monier-Williams import is inside the function that needs it and raises
saying what to install. There are no extras.

## Test with real verses

The metre assertions use Meghadūta 1.1 and Gītā 1.1. A metre detector that passes on invented
syllables has not been tested.

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
