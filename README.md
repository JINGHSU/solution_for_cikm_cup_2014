Fancyspeed's solution for CIKM2014 Cup (the 5th place).
===================================================

## Background

The task is query classification, or query intent detection. 

About the competition, please visit http://cikm2014.fudan.edu.cn/index.php/Index/index and http://openresearch.baidu.com/topic/71.jspx


## Challenges

* Multi-class multi-label
* Short text
* Click and session
* Unlabelled data
* Unbalanced data

## Ideas

* Structured labels
* N-gram, word position, aggregated query as a sample
* In-session queries and labels, keyword and entity detection
* Semi-supervised learning
* Sampling, post-processin

## Features

* query words (1-gram, 2-gram, word position)
* clicked title words (1-gram, 2-gram)
* words of top 30 titles in query's same sessions
* words of top 3 labels in query's same sessions
* labels in query's same sessions
* query length
* query frequence
* average length of clicked titles
* average search times in query's same sessions
* average click times in query's same sessions
* averge duplicated clicks in query's same sessions

## Methods and tools

* GBM: Xgboost with softmax-objective
* SVC: Liblinear
* Multi-class LR: Sklearn.MultiTaskLasso
* Random Forest: Sklearn.RandomForestClassifier
* Labelled LDA: modified PLDA
* Markov Chain: query-query similarity by text and session co-occurrence

## Ensemble

* weighted averaging
* linear model
* cascading: feed xgboost

## Post-processing

* Calibration: same label distribution as training set
* Threshold: same average labels as training set

## How to run

  * Dependencies:
    1. XGBoost for GBT: https://github.com/tqchen/xgboost
    2. Liblinear for LR 

  * Assumpation:
    1. XGboost's path is ../../tools/xgboost3/ 
    2. Liblinear's path is ../../tools/liblinear/ 
    3. raw training data is in ../raw_data
    4. need 3 folds ../trans_data, ../dataset, ../submit for temporary data

  * Steps:
    1. reorganize train.txt: refine_train_by_sesson_query.py 
    2. merge information for each query: trans_train1.py and trans_train2.py
    3. generate features: prepare1.py
    4. train: run_xgboost3_pig.sh
    5. train: run_liblinear_pig.sh
    6. ensemble: run_average.sh

