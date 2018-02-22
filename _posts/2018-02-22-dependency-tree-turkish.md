It is very easy to use the [Jupyter Notebook](http://jupyter.org/) with nbextensions for visualazing the tests of spaCy model. The [displaCy Jupyter extension](https://github.com/explosion/spacy-dev-resources/tree/master/jupyter-displacy) is a simple extension for [Jupyter Notebook](http://jupyter.org/) that lets you visualize a JSON-formatted dependency parse using the [displaCy visualizer](https://demos.explosion.ai/displacy/).


1. [Install Jupyter Notebook](http://jupyter.org/install).

2. Install Jupyter [nbextension](https://github.com/ipython-contrib/jupyter_contrib_nbextensions).
```bash
pip install jupyter_contrib_nbextensions
```

3. Install [displaCy Jupyter extension](https://github.com/explosion/spacy-dev-resources/tree/master/jupyter-displacy).
```bash
jupyter nbextension install --user https://github.com/explosion/spacy-dev-resources/tree/master/jupyter-displacy
```

4. Enable the extension.
```bash
jupyter nbextension enable displacy
```

5. Run the [Jupyter Notebook](http://jupyter.org/) and create new notebook in browser window. Import *spacy*, *displacy* and load *tr_unnamed model*.
```python
import spacy
from spacy import displacy
nlp = spacy.load('tr_unnamed')
```

6. Process whole sentence and visualize the results.
```python
doc1 = nlp('Ben seni çok seviyorum.')
for token in doc1:
    print(token.text, token.lemma_, token.pos_, token.tag_)
displacy.render(doc1, style='dep', jupyter=True)
```

 The results of sentence processing:
```
Ben Ben PRON Pers
seni seni PRON Pers
çok çok ADV Adverb
seviyorum sev VERB Verb
. . PUNCT Punc
```

![alt text](../media/images/bensenicokseviyorum.jpg "Tree")

