---
layout: post
title:  Adding a SpaCy support for Turkish. Part 2
excerpt_separator: <!--more-->
---
Before making a decision if we can add a support for Turkish language we need to check availability of basic training data.
We went through the publicly available Turkish corpora and analysed this.

<!--more-->

### Available Turkish data for training spaCy models.

[CoNLL-U](http://universaldependencies.org/format.html) is the format used by the Universal Dependencies initiative to annotate dependency [treebanks](https://en.wikipedia.org/wiki/Treebank). A large number of treebanks for many different languages are available in the [CoNLL-U](http://universaldependencies.org/format.html) format.

Sentences consist of one or more word lines, and word lines contain the following fields:
   1. ID: Word index, integer starting at 1 for each new sentence; may be a range for multiword tokens; may be a decimal number for empty nodes.
   2. FORM: Word form or punctuation symbol.
   3. LEMMA: Lemma or stem of word form.
   4. UPOSTAG: Universal part-of-speech tag.
   5. XPOSTAG: Language-specific part-of-speech tag; underscore if not available.
   6. FEATS: List of morphological features from the universal feature inventory or from a defined language-specific extension; underscore if not available.
   7. HEAD: Head of the current word, which is either a value of ID or zero (0).
   8. DEPREL: Universal dependency relation to the HEAD (root iff HEAD = 0) or a defined language-specific subtype of one.
   9. DEPS: Enhanced dependency graph in the form of a list of head-deprel pairs.
   10. MISC: Any other annotation.

The main problem in training spaCy models is obtaining threebanks data in [CoNLL-U](http://universaldependencies.org/format.html) format. Source treebanks can be obtained only by an nonautomated way, by expert linguistic analysis. The small treebanks can be obtained by [Universal Dependencies](http://universaldependencies.org/).

[Universal Dependencies](http://universaldependencies.org/) (UD) is a framework for cross-linguistically consistent grammatical annotation and an open community effort with over 200 contributors producing more than 100 treebanks in over 60 languages. [The UD Turkish Treebank](https://github.com/UniversalDependencies/UD_Turkish), also called the IMST-UD Treebank, is a semi-automatic conversion of the [IMST Treebank](https://web.itu.edu.tr/gulsenc/papers/turcling2016_treebank.pdf) (Sulubacak et al., 2016), which is itself a reannotated version of the METU-Sabancı Turkish Treebank (Oflazer et al., 2003). All three of the treebanks share the same raw data, a set of 5635 sentences collected from daily news reports and novels.

The spaCy [convert](https://spacy.io/api/cli#convert) command helps data from *.conllu to convert [the UD Turkish Treebank](https://github.com/UniversalDependencies/UD_Turkish) data to [JSON](https://spacy.io/api/annotation#json-input) format for training.

```python
python -m spacy convert tr-ud-dev.conllu  ./ -c conllu
python -m spacy convert tr-ud-train.conllu  ./ -c conll
python -m spacy convert tr-ud-test.conllu  ./ -c conll
```
The following results can be obtained: [tr-ud-dev.conllu](https://github.com/UniversalDependencies/UD_Turkish/blob/master/tr-ud-dev.conllu) gives 975 trees, [tr-ud-train.conllu](https://github.com/UniversalDependencies/UD_Turkish/blob/master/tr-ud-train.conllu) gives 3685 trees, [tr-ud-test.conllu](https://github.com/UniversalDependencies/UD_Turkish/blob/master/tr-ud-test.conllu) gives 975 trees.

An example of [JSON](https://spacy.io/api/annotation#json-input) file structure:

```json
 {
    "id":3,
    "paragraphs":[
      {
        "sentences":[
          {
            "tokens":[
              {
                "head":2,
                "tag":"Noun",
                "orth":"Orada",
                "dep":"obl"
              },
              {
                "head":-1,
                "tag":"Rel",
                "orth":"ki",
                "dep":"case"
              },
              {
                "head":2,
                "tag":"Verb",
                "orth":"tart\u0131\u015fma",
                "dep":"nsubj"
              },
              {
                "head":1,
                "tag":"Adverb",
                "orth":"hayli",
                "dep":"advmod"
              },
              {
                "head":0,
                "tag":"Adj",
                "orth":"zengin",
                "dep":"ROOT"
              },
              {
                "head":-1,
                "tag":"Punc",
                "orth":".",
                "dep":"punct"
              }
            ]
          }
        ]
      }
    ]
  }
```