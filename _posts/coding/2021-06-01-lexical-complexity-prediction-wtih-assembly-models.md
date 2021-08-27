---
layout: single
title: Lexical Complexity Prediction with Assembly Models
date: 01 June 2021
author_profile: true
share: false
categories: coding
show_date: true
---

This is my Undergraduate Honors Thesis for the major in Computer Science at Dartmouth College. This research was conducted under the invaluable mentorship of Master's student Weicheng Ma and direction of Professor Soroush Vosoughi, who made this journey into natural language processing, into what potentially makes language difficult to understand, possible.

Unfortunately, my entire work won't render properly on this webpage as it was written in LaTeX...

Fortunately, there are ways you can check it out elsewhere!

## Way #1: Abstract (TLDR)

Tuning the complexity of one's writing is essential to presenting ideas in a logical, intuitive manner to audiences. This paper describes a system submitted by team `BigGreen` to LCP 2021 for predicting the lexical complexity of English words in a given context. We assemble a feature engineering-based model and a deep neural network model with an underlying Transformer architecture based on BERT. While BERT itself performs competitively, our feature engineering-based model helps in extreme cases, eg. separating instances of easy and neutral difficulty. Our handcrafted features comprise a breadth of lexical, semantic, syntactic, and novel phonetic measures. Visualizations of BERT attention maps offer insight into potential features that Transformers models may implicitly learn when fine-tuned for the purposes of lexical complexity prediction. Our assembly technique performs reasonably well at predicting the complexities of single words, and we demonstrate how such techniques can be harnessed to perform well when on multi word expressions (MWEs) too.

## Way #2: Shallow Dive (15 minute read)

You can find my work published in the _Proceedings of the 15th International Workshop on Semantic Evaluation (SemEval-2021)_ [here](https://aclanthology.org/2021.semeval-1.86/).

## Way #3: Deep Dive (40 minute read)

You can find my full thesis via Dartmouth Digital Commons (DDC) [here](https://digitalcommons.dartmouth.edu/senior_theses/222/).

## Way #4: GitHub

The code may be too much to take in, but if you're curious you can find all of it right [here](https://github.com/Aadil101/BigGreen-at-LCP-2021.git).