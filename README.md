# ganapati

Sanskrit philology for a search index. Metre, verse chunking, citations, lemmas and glosses.

```python
import ganapati                       # registers the Sanskrit profiles with litesearch
from litesearch import Index

ix = Index('vault.db')
ix.add_file('mahabharata_u.htm')      # verse-sized chunks, a tree built from the citations
ix.db.by_meter(meter='mandākrāntā')   # every verse in that metre
```

## What it does

`import ganapati` teaches litesearch to read a Sanskrit source. Before it, a GRETIL Manusmṛti of
306,920 characters became one tree node and chunks cut mid-verse. After it, the citation is the
chunk boundary, the chunk id, and the address the tree is built from.

Each chunk also carries its metre as searchable metadata:

```python
from ganapati import verse_meta

verse_meta('dharmakṣetre kurukṣetre samavetā yuyutsavaḥ '
           'māmakāḥ pāṇḍavāścaiva kimakurvata sañjaya')
# {'meter': 'anuṣṭubh', 'variant': 'pathyā', 'gana': 'ma_ra_ga_ga', 'pada': '8'}
```

## The three modules

| module | what is in it |
|----|----|
| `ganapati.text` | verse boundaries, the two chunkers, readers for GRETIL, TEI, VR XML and DCS |
| `ganapati.metre` | IAST transliteration, syllable weights, the gaṇas, 80 metres, mātrā metres |
| `ganapati.lemma` | vidyut lemmas, Monier-Williams glosses, and the litesearch profiles |

## Install

```
pip install ganapati
```

Lemmas and glosses need `vidyut` and an 81 MB data download, both reached on first use. Without
them a store still gets metre, and `sanskrit_meta(None)` is `verse_meta` itself.

## What stayed in litesearch

The FTS5 tokenizer that folds Devanagari and IAST to one key. It is on for every store, not only
Sanskrit ones, so it belongs with the index rather than here.
