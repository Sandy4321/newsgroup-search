# newsgroup-search

A simple search engine for the 20 newsgroups text dataset. 

Support for boolean queries: AND, OR
------------------------------------

An implementation of the boolean retrieval model based on an inverted index. 
The words in the documents belonging to the 20 newsgroups corpus represent 
the terms in the inverted index. To each term a document id list is assigned, 
representing the ids of documents where the word appears. The inverted index 
is stored in memory (both the terms and the list of document ids 
corresponding to each term).

Another option (not explored here) would have been, instead of using words in 
the collection for the terms of the inverted index, to use the newsgroup 
assigned to each document - it is unclear if the search is to be performed 
based on the newsgroup or based on words in the corpus.

To support boolean queries each query is converted to a syntax tree (using 
the ast python package). The tree is recursively traversed in order to obtain
 the query result. 

A tf-idf-based ranker for simple queries
----------------------------------------

The 20 newsgroups corpus is converted to a sparse tfidf matrix where the rows 
are the documents and the columns are the terms present in the corpus (using 
scikit-learn's TfidfVectorizer).
  
A given query is converted to a sparse vector based on the tfidf matrix 
previously obtained. This vector will have values of 0 but for the terms 
which match the terms in the matrix. The rows (corresponding to document ids) 
of these non-zero terms are retrieved and ordered by the tfidf weight. The 
top k documents are returned as a result of the query.

Improvements
------------

* enhance the pre-processing steps: tokenization, normalization, handling of 
punctuation

* for the inverted index, store the list of document ids on disk (not 
necessary in this example as the size of the collection permits in-memory 
storage).

* use a state-of-the-art search engine such as Elasticsearch (based on Lucene)

* account for the meaning of words in context, as querying for 'kiwi bird' 
should not yield results which are relevant for fruits. This can be achieved 
by changing the current bag-of-words model with a model such as word2vec or 
similar Neural word representations which can model semantic and syntactic 
word relationships

Usage
-----

`python newsgroup.py`

To create the conda environment

`conda create -p env --file conda.txt`

`source activate env/`

