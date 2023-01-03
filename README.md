# AAAI2023
## Introduction

This program include the following file structure:

- [folder] input : input files includes source data, preprocessed data and some embedding like w2v
  - [folder] cache：some intermediate data cached 
- [folder] output: the output of each kfold folder, for collecting several output and make a blending.
- [folder] submission: the output of final results.
- [file] preprocessing.ipynb: preprocess and save the intermediate data to the folder "input"
- [file] w2v_emb.ipynb: use word2vec to generate question embedding
- [file] model.ipynb: the main file of the model

It requires 128G memory, and since the use of ensemble, the whole "main.ipynb" need to run several days.

## Main Idea

1. Preprocess the data by using "preprocessing.ipynb"： dule wtih the source data of sequence into lines (key: uid, question, timestamp)
1. Do feature engineering by using "model.ipynb" in chapter 1.1 to 3.4， and get question embedding by using "w2v_emb.ipynb"
1. For the purpose of smaller distance to the test set, we use stratified kfold to split the train set by "uid", according to the sequence length of each uid.
1. Randomly split the label bound of visible and disvisible, and we can have different sequence to generate features, which is a kind of data augmentation.
1. Train the lightgbm model by using "model.ipynb" in chapter 4.1, and use the training weight as the "weight_func", for the purpose of smaller "uid accuracy" distance to the test set. 
1. Use three different sets of features、two different sets of lightgbm parameters to make a blending.
1. Only a little post process according to characteristics (very very small promote on public leaderboard):
   - the same question with same uid and timestamp would always have the same label, when half of label is visible while another half is disvisible.

## Feature Engineering

- Using word2vec to get question embedding
- Generate some global features ahead of data augmentation for saving time (including features with no concerned on "uid")
- Generate some features with "uid" after data augmentation.
