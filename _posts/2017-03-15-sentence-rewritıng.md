---
layout: post
title:  English Sentence Rewriting Model Design And Training.
---

Technical implementation of a sentence re-writer based on seq2seq encoder can be taken from TensorFlow.  - software designed by google which simplifies neural networks building and training.


###### Solving process implies following stages and steps:
1. Data obtaining and preprocessing
2. Neural network training
3. Model evaluation
Let’s go through the steps in details. In order to train most of neural network based models we need quite large dataset. In textual case training data represented as a thing typically called “corpus”. 
We chose gutenberg corpus which contains 54000 free e-books from different authors. The data preprocessing sequence:
1. Sentence extraction from a “raw” text. To do this we rely on Beautiful Soap
2. Run over the “unique” filter
3. Random shuffling and saving to a separate file
4. Running through the POS tagging procedure (internally we use Spacy for this)

   |TAG|
   |-----|
   |_NOUN|
   |_ADJ|
   |_VERB|
   |_ADV|
   |_PRON|
	
1. The adjective filtration in templates is done using following algorithm:
   1. Several _ADJ tags are technically mean one complex _ADJ, so that we substitute the adjectives sequence by one _ADJ tag.
   2. All the templates with absolute frequency above 9 are saved in a separate file - templates bank.
   3. In our case these steps are controlled by bash script that looks like this:
        ```
        #! /bin/bash
        ```
###### Extract sentences from raw text.
```
echo "Extracting sentences in progres."
python extractor.py --gutenberg ./gutenberg/A
echo '\n'
```
###### Uniqualization of the received sentences.
```
python uniqueizer.py sentences.txt
echo "Sentences uniqalization in progres."
```
###### Shuffle lines in file.
```
shuf output_uniq.txt > output_shuf.txt
echo "Extracting tags in progres."
```
###### Extract all tags from sentences. 
```
python tags_extractor.py < output_shuf.txt > output_tags.txt
total_lines=$(wc -l < output_shuf.txt)
echo "Total lines ${total_lines}."
```
###### Filtering ADJ. Multi-ADJ is equal to one ADJ. Counting frequencies. Minimum frequency is 10.
```
python templates.py 10 ${total_lines} < output_tags.txt
echo '\n'
echo 'All done!\n'
```
1. Templates filtering by occurrence. The most important templates have length 5-16 tags. The output of our script:
```
There are 16236 templates 9 tags.
There are 15804 templates 8 tags.
There are 13019 templates 10 tags.
There are 11207 templates 7 tags.
There are 8305 templates 11 tags.
There are 5611 templates 6 tags.
There are 4576 templates 12 tags.
There are 2024 templates 13 tags.
There are 1861 templates 5 tags.
There are 786 templates 14 tags.
There are 244 templates 15 tags.
There are 60 templates 16 tags.
```
1. Applying of a templates filter to sentences set we have (all the sentences with templates not represented in a templates file are ignored). This is expected to be a long running operation, but parallelizable. We use several cores to run this.
2. At this point we have an output that represents required input data with correctly built sentences. Input sequence is prepared  according to selected strategy. Proper names are substituted by _NAME1, _NAME2, _NAME3 tags
   1. Phrasal verbs need a specific treatment - we need them to be considered as a one word/one tag _VERB. To bo this, in a preprocessing stage we join multiword verbs into one like this: “give up” == “give_up”. Finally we’ll get the single _VERB tag for all the sequence.
   2. Adjective synonymization (_ADJ) using word net.
   3. All non-adjective words are lemmatized with in 50% probability
   4. Adjective synonymization (_ADJ) using word net. The probability of synonymization is about 0.4 or 0.5 as a hyperparameter
   5. The most significant and important words are _NOUN, _ADJ, _VERB, _PRON are added into input sequence in a lemmatized form. 
   6. Lemmatized words and templates are represented with an equal probability (0,5)
   7. Proper names tag (NAME1, NAME2 … NAMEN) are added with 100% probability, other words are lemmatized 
This strategy leads us to preprocess training data under 6 outputs for each input. This means that we have 6 times data increase and additional diversity adding.
1. Split data into training and test sets 80% + 20%.
Once we have data we can train the model (python main.py). It ıs based on pretty well know seq2seq encoder frequently used for machine translation. We train different models for different groups of templates, united by length (buckets = [(7, 7), (10, 10), (13, 13), (16, 16)]).


Classic illustration of RNN (image from the internet):
  



And its LSTM version used fo:
  



|       Model can be read in tensor flow:       |
|-------------------------------------------------|
|[https://www.tensorflow.org/tutorials/recurrent] |
|[https://www.tensorflow.org/tutorials/seq2seq]|


|Additional links:|
|----------------------------------------------------------|
|[http://colah.github.io/posts/2015-08-Understanding-LSTMs/]|
|[https://habrahabr.ru/company/wunderfund/blog/330194/]|
|[https://habrahabr.ru/company/wunderfund/blog/331310/]|
|[http://www.bioinf.jku.at/publications/older/2604.pdf]|
|[https://arxiv.org/pdf/1409.3215.pdf]|
|[https://www.gutenberg.org/]|