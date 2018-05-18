---
layout: post
title:  Visualize parsing result
---
This is just a primitive "setup and use" snippet.


It is very easy to use the [Jupyter Notebook](http://jupyter.org/) with nbextensions for visualazing the tests of spaCy model. 
The [displaCy Jupyter extension](https://github.com/explosion/spacy-dev-resources/tree/master/jupyter-displacy) is a 
simple extension for [Jupyter Notebook](http://jupyter.org/) that lets you visualize a JSON-formatted dependency 
parse using the [displaCy visualizer](https://demos.explosion.ai/displacy/).

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

5. Process whole sentence and visualize the results.
```python
    doc1 = nlp('Dependency tree parsing results can be visualized easily in a notebook.')
    for token in doc1:
        print(token.text, token.lemma_, token.pos_, token.tag_)
    displacy.render(doc1, style='dep', jupyter=True)
```

<img src="http://blog.ai-labs.org/media/images/parsing_results.svg" width="100%">

See: [SpaCy Visualizers](https://spacy.io/usage/visualizers)

See: [Possible visualizer options](https://spacy.io/api/top-level#displacy_options)