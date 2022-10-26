# EDFC

Code and dataset for paper "A Topic-Enhanced Approach for Emotion Distribution Forecasting in Conversations".

Meanwhile, we present the details of the dataset construction here.

## Dataset

Although the EDFC task is more reasonable and we have proposed an approach to alleviate the need for emotion distribution annotations during training, it still has the problem of difficult evaluation during testing. 
Specifically, **the validation and test sets of existing datasets lack emotion distribution annotations**, which makes sample-level evaluation and error analysis impossible.
We construct two new datasets: a single-turn Weibo dataset and a multi-turn Douban dataset. 
Combining these two datasets, we can greatly alleviate the above problem and achieve a more comprehensive evaluation. 

### Weibo Dataset

The Weibo dataset is constructed based on one-to-many Weibo Conversation Corpus[1]. 
The original dataset consists of a large number of single-turn dialogues, in which the same query corresponds to an average of 20 responses, which gives us the possibility to count the emotion distribution of next utterance.

Specifically, we first train an emotion classifier using the NLPCC dataset, which is from the Emotion Analysis in Chinese Weibo Texts task in NLPCC2013 and NLPCC2014. 
Our classifier is a fine-tuned RoBERTa-base model whose parameters are derived from Chinese RoBERTa[2], and obtains the accuracy of 91.67% and the macro-F1 of 78.56% on the divided test set. 
Then, we annotate emotion labels for all responses in the one-to-many Weibo Conversation Corpus using the classifier automatically, so that **we can obtain the approximate emotion distribution of next utterance for each query by statistics.**
Finally, we also probabilistically sample an emotion label from the emotion distribution for each query. 
So far, for each query in the dataset, we obtain an emotion label and emotion distribution of next utterance.

The division is shown in Table 1. 
In addition to training set, validation set and test set, **we also added test m-2 set and test m-3 set.**
These test sets are designed to evaluate one-to-many performance, which select samples with high probability of two emotions and three emotions at the same time in emotion distribution, respectively.

The symbol **e** marked by * in Table 1 has different meanings under different datasets, in this dataset it can be either an emotion label or an emotion distribution. 
The symbol **x** marked by # is similar, in this dataset it represents other dialogs and utterances. 
This dataset contains five emotion categories: neutral, happiness, sadness, anger and disgust. 
More statistics are shown in Table 2, and the scope of statistics is those responses that are automatically annotated using the emotion classifier when we obtain the approximate emotion distribution. 

**Note that it is an ideal dataset for evaluation friendly.**
Although this dataset has emotion distribution annotations, we only use them for evaluation. 
While training, we still only use emotion labels to simulate the learning dilemma in most real dialogue scenarios. 

### Douban Dataset

The Douban dataset is constructed based on Douban Conversation Corpus[3]. 
The original dataset contains a large number of multi-turn dialogues, and we annotate utterance-level emotion labels manually for some dialogues. 
Specifically, we give annotation guidelines and examples to three human annotators from a data labeling company, and we vote on their annotation results to obtain final emotion labels. 

The division of the Douban dataset is shown in Table 1. 
The symbol **e** marked by * has different meanings under different datasets, in this dataset it can only be an emotion label. 
The symbol **x** marked by # is similar, in this dataset it only represents other dialogs. 
This dataset contains five emotion categories: neutral, happiness, sadness, anger and disgust. 
More detailed statistics are shown in Table 2, and the scope of our statistics is utterances with manual emotion label annotations. 

**Note that it is a dataset closer to the real scene**, but it can only be evaluated by perplexity.

## References

[1] Jun Gao, Wei Bi, Xiaojiang Liu, Junhui Li, and Shuming Shi, “Generating multiple diverse responses for shorttext conversation,” Proceedings of the AAAI Conference on Artificial Intelligence, vol. 33, no. 01, pp. 6383–6390, Jul. 2019.

[2] Yinhan Liu, Myle Ott, Naman Goyal, Jingfei Du, Mandar Joshi, Danqi Chen, Omer Levy, Mike Lewis, Luke Zettlemoyer, and Veselin Stoyanov, “Roberta: A robustly optimized bert pretraining approach,” ArXiv, vol. abs/1907.11692, 2019.

[3] Yu Wu, Wei Wu, Chen Xing, Ming Zhou, and Zhoujun Li, “Sequential matching network: A new architecture for multi-turn response selection in retrieval-based chatbots,” in Proc. of ACL, 2017.


