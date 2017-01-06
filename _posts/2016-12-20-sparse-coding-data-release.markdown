---
layout: post
title:  "Sparse Coding of Neural Word Embeddings for Multilingual Sequence Labeling"
date:   2016-12-20 00:10:30 +0100
comments: true
permalink: sparse_embeds
---
This page contains the sparse word representations used for the experiments of the TACL paper entitled *Sparse Coding of Neural Word Embeddings for Multilingual Sequence Labeling*.

We determined *sparse* word representations based on the dense distributed word representations of the [polyglot project](https://sites.google.com/site/rmyeid/projects/polyglot) using the objective function

$$\min\limits_{D \in \mathcal{C}, \alpha} \frac{1}{2n} \sum_{i=1}^{n} \left(\lVert \mathbf{x}_i-D \boldsymbol{\alpha}_i \rVert_2^2 + \lambda \lVert \boldsymbol{\alpha}_i \rVert_1 \right),$$

where $D$ belongs to the convex set of matrices comprising of unit vectors, $\mathbf{x}_i$ refers to a dense polyglot word representation and $\boldsymbol{\alpha}_i$ corresponds to its sparse counterpart. The files below contain a `scipy.sparse.csr_matrix` object storing an $\alpha \in \mathbb{R}^{\lvert V \rvert \times 1024}$ sparse matrix for each language (with $\lvert V \rvert$ denoting the size of the vocabulary).
    
The vocabulary for each language is tied to that of the original polyglot representations, so in order to figure out which row of $\alpha$ corresponds to which vocabulary unit, one should obtain the original polyglot vocabularies. The vocabularies can be downloaded from the [project website](https://sites.google.com/site/rmyeid/projects/polyglot#TOC-Download-the-Embeddings) (together with the dense embeddings themselves). Loading the vocabulary and both the sparse and dense embeddings thus can be performed via:

```python
import pickle
vocabulary, dense_polyglot_embeddings = pickle.load(open(path_to_dense_polyglot_embeddings, 'rb'))
sparse_embeddings = pickle.load(open(path_to_alphas_file, 'rb'))
```

{% assign languages = "bg,cs,da,de,el,en,es,et,eu,fa,fi,fr,ga,he,hi,hr,hu,id,it,la,nl,no,pl,pt,ro,sl,sv,ta,tr" | split: "," %}
{% assign conllx_languages = "bg,da,de,en,es,hu,it,nl,pt,sl,sv,tr" | split: "," %}
|-----------------------+--------------+-------------+-------------+-------------+-------------+-------------|
|Wikipedia language code|$\lambda=0.05$|$\lambda=0.1$|$\lambda=0.2$|$\lambda=0.3$|$\lambda=0.4$|$\lambda=0.5$|
|-----------------------+--------------+-------------+-------------+-------------+-------------+-------------|
{% for l in languages %}|{{ l }}|{% if conllx_languages contains l %}[{{ l }}-0.05](http://rgai.inf.u-szeged.hu/~berend/sparse_reps/{{ l }}-1024-0.05.alph){% else %}---{% endif %}|[{{ l }}-0.1](http://rgai.inf.u-szeged.hu/~berend/sparse_reps/{{ l }}-1024-0.1.alph)|[{{ l }}-0.2](http://rgai.inf.u-szeged.hu/~berend/sparse_reps/{{ l }}-1024-0.2.alph)|[{{ l }}-0.3](http://rgai.inf.u-szeged.hu/~berend/sparse_reps/{{ l }}-1024-0.3.alph)|[{{ l }}-0.4](http://rgai.inf.u-szeged.hu/~berend/sparse_reps/{{ l }}-1024-0.4.alph)|[{{ l }}-0.5](http://rgai.inf.u-szeged.hu/~berend/sparse_reps/{{ l }}-1024-0.5.alph)|
{% endfor %}

We also created word representations using the objective functions introduced by [Faruqui et al. (2015)](https://aclweb.org/anthology/P/P15/P15-1144.pdf). This approach comes in two flavors (using an unconstrained and a non-negativity constrained objective function). For more details please refer to the paper.

{% assign languages = "bg,cs,da,de,el,en,es,et,eu,fa,fi,fr,ga,he,hi,hr,hu,id,it,la,nl,no,pl,pt,ro,sl,sv,ta,tr" | split: "," %}
|Wikipedia language code|uncostrained|non-negative|
{% for l in languages %}|{{ l }}|[{{ l }}-0.5](http://rgai.inf.u-szeged.hu/~berend/sparse_reps_unc/{{ l }}-1024-0.5.alph)|[{{ l }}-0.5](http://rgai.inf.u-szeged.hu/~berend/sparse_reps_nn/{{ l }}-1024-0.5.alph)|
{% endfor %}
