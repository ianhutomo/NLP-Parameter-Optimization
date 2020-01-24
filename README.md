# Search Function Parameter Optimization

This code is part of the assignment on Text Information Systems course at the University of Illinois at Urbana-Champaign

In the final part of MP2, you will participate in a search competition where you will create a Search Engine using MeTA, similar to what you did for Part 2. Your ranker will be evaluated using NDCG scores on 3 relevance datasets: Cranfield dataset, APNews dataset, and the Faculty dataset collected and annotated by you and your classmates. 

**The evaluation results will be displayed on the leaderboard: [LINK COMING SOON]**

Also, you are free to edit all files **except**:
- .gitlab-ci.yml
- timeout.py 
- competition.py

## Due: ~~Oct 13, 2019~~ Oct 20, 2019 at 11:59 pm

**NOTE:** If you've completed Part2, you should be familiar with the basics: Setup, Indexing and Searching the Data. We've included these sections in this README again for convenience. So, feel free to skip directly to the Competition Tasks section!

## Setup

We'll use [metapy](https://github.com/meta-toolkit/metapy)---Python bindings for MeTA. 
If you have not installed metapy so far, use the following commands to get started.

```bash
# Ensure your pip is up to date
pip install --upgrade pip

# install metapy!
pip install metapy pytoml
```

If you're on an EWS machine
```bash
module load python3
# install metapy on your local directory
pip install metapy pytoml --user
```

Read the [C++ Search Tutorial](https://meta-toolkit.org/search-tutorial.html). Read *Initially setting up the config file and Relevance judgements*.
Read the [python Search Tutorial](https://github.com/meta-toolkit/metapy/blob/master/tutorials/2-search-and-ir-eval.ipynb)

If you cloned this repo correctly, your assignment directory should look like this:
- MP2_part4/: assignment folder
- MP2_part4/cranfield/: Cranfield dataset in MeTA format.
- MP2_part4/cranfield-queries.txt: Queries one per line, copy it from the cranfield directory.
- MP2_part4/cranfield-qrels.txt: Relevance judgements for the queries, copy it from the cranfield directory.
- MP2_part4/stopwords.txt: A file containing stopwords that will not be indexed.
- MP2_part4/config.toml: A config file with paths set to all the above files, including index and ranker settings.

## Indexing the data
To index the data using metapy, you can use either Python 2 or 3.
```python
import metapy
idx = metapy.index.make_inverted_index('config.toml')
```

## Search the index
You can examine the data inside the cranfield directory to get a sense about the dataset and the queries.

To examine the index we built from the previous section. You can use metapy's functions.

```python
# Examine number of documents
idx.num_docs()
# Number of unique terms in the dataset
idx.unique_terms()
# The average document length
idx.avg_doc_length()
# The total number of terms
idx.total_corpus_terms()
```

Here is a list of all the rankers in MeTA.Viewing the class comment in the header files shows the optional parameters you can set in the config file:

- [Okapi BM25](https://github.com/meta-toolkit/meta/blob/master/include/meta/index/ranker/okapi_bm25.h), method = "**bm25**" 
- [Pivoted Length Normalization](https://github.com/meta-toolkit/meta/blob/master/include/meta/index/ranker/pivoted_length.h), method = "**pivoted-length**"
- [Absolute Discount Smoothing](https://github.com/meta-toolkit/meta/blob/master/include/meta/index/ranker/absolute_discount.h), method = "**absolute-discount**"
- [Jelinek-Mercer Smoothing](https://github.com/meta-toolkit/meta/blob/master/include/meta/index/ranker/jelinek_mercer.h), method = "**jelinek-mercer**"
- [Dirichlet Prior Smoothing](https://github.com/meta-toolkit/meta/blob/master/include/meta/index/ranker/dirichlet_prior.h), method = "**dirichlet-prior**"

In metapy, the rankers can be called as:

```python
metapy.index.OkapiBM25(k1, b, k3) where k1, b, k3 are function arguments, e.g. ranker = metapy.index.OkapiBM25(k1=1.2,b=0.75,k3=500)
metapy.index.PivotedLength(s) 
metapy.index.AbsoluteDiscount(delta)
metapy.index.JelinekMercer(lambda)
metapy.index.DirichletPrior(mu)
```
## Competition Tasks
For your code to be ranked in the leaderboard, you will need to follow the instructions in `GitLab_Competition_Instructions.pdf`.

*search_eval.py* contains some starter code to evaluate the performance of the OkapiBM25 ranker on the cranfield dataset using NDCG. You should modify this file for the competition. 

You are free to use any metapy ranker, fine-tune various parameter settings or even your use your own implementation of ranking functions. Feel free to improvise and create your own rankers! You may use the provided cranfield dataset to evaluate your rankers/parameter settings locally but remember that the leaderboard ranking is based on the performance on all the 3 datasets, so please make sure you do not overfit. 

To see how well you perform in the leaderboard, you need to first submit your choice in GitLab by editing the **load_ranker** function inside **search_eval.py** to return the ranker of your choice.There are no restrictions on the number of submissions.

## Grading

You must beat the baseline on the leaderboard to get full credit.

## Bonus: Top ranked in Search Competition Leaderboard

Try to get a top position in the competition leaderboard. The higher your rank is, the more extra credit you will receive.
Our grading formula for the competition is  5-(Rank-1)/10, where Rank is the position of the student.