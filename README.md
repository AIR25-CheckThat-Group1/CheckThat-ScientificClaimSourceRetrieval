## Project Overview - Experimented by Onbas

This repository contains the implementation of various experimental approaches for the Scientific Claim Source Retrieval task. The goal is to retrieve scientific papers mentioned in social media posts (tweets) using advanced NLP and information retrieval techniques.

This project implements and evaluates several approaches to improve the retrieval of scientific papers from social media posts:

1. **Bi-Encoder Architecture**: A dual-encoder model using Sentence-BERT for semantic matching
2. **Query Expansion**: Techniques to enhance tweet queries with relevant scientific terms
3. **Multi-Hop Fusion**: Multi-step retrieval process for complex relationships
4. **Hybrid Retrieval**: Combination of Multi-Hop Fusion and Query Expansion

The implementation includes hyperparameter tuning and evaluation using MRR@5 metric.

## Pre-trained Model Examinations

### Sentence-BERT Models
We evaluated several pre-trained Sentence-BERT models for select our base model:

1. **all-MiniLM-L6-v2**
2. **multi-qa-MiniLM-L6-cos-v1**
3. **multi-qa-MiniLM-L6-dot-v1**
4. **allenai/specter**
5. **msmarco-distilbert-base-tas-b**
6. **multi-qa-mpnet-base-cos-v1**

Model Selection Criteria: Base model performance on retrieval task.

## Hyperparameter Tuning

1. **The effect of different learning rates and batch sizes**
2. **The effect of different loss functions on SBERT(not fully evaluated due to performance drops, and already pre-trained with MultipleNegativesRankingLoss)**
3. **The effect of different data augmentation methods(text cleaning)**

# Subtask 4b (Scientific Claim Source Retrieval)
Given an implicit reference to a scientific paper, i.e., a social media post (tweet) that mentions a research publication without a URL, retrieve the mentioned paper from a pool of candidate papers.

The task is a retrieval task, where the query set contains tweets mentioning papers implicitly, and the collection set contains the pool of papers mentioned by tweets.

__Table of contents:__

<!-- - [Evaluation Results](#evaluation-results) -->
- [List of Versions](#list-of-versions)
- [Contents of the Task 1 Directory](#contents-of-the-repository)
- [Datasets statistics](#datasets-statistics)
- [Input Data Format](#input-data-format)
- [Output Data Format](#output-data-format)
- [Evaluation Metrics](#evaluation-metrics)
- [Scorers](#scorers)
- [Baselines](#baselines)
- [Credits](#credits)

<!-- ## Evaluation Results

TBA -->

## List of Versions
- [15/01/2025] Training data released.

## Contents of the Subtask4b Directory

This repository contains the following files:

1. **getting_started_subtask4b.ipynb** Jupyter notebook that enables participants of subtask 4b to quickly get started
2. **subtask4b_collection_data.pkl** The collection set for the retrieval task (CORD-19 academic papers' metadata)
3. **subtask4b_query_tweets_train.tsv** The query set (train split) for the retrieval task (tweets with implicit references to CORD-19 papers)
4. **subtask4b_query_tweets_dev.tsv** The query set (dev split) for the retrieval task (tweets with implicit references to CORD-19 papers)
4. **README.md** this file

## Datasets statistics
The data for the retrieval task is made of two sets:
* **Query set (train & dev splits)**
  * content: tweets with implicit references to scientific papers from CORD-19.
  * stats: 14,253 tweets.
* **Collection set**
  * content: metadata of the CORD-19 scientific papers which tweets implicitly refer to.
  * stats: metadata of 7,764 scientific papers.

## Input Data Format
### Query set (train & dev splits)

The tweets are provided as a TSV file with three columns:
> post_id <TAB> tweet_text <TAB> cord_uid

Where: <br>
* post_id: a unique identifier for the tweet. Ids were generated for easier evaluation of submissions
* tweet_text: the tweets' texts
* cord_uid: the label to be predicted, which is the unique identifier of the scientific paper that the tweet refers to

**Example:**
See Subtask4b examples at: https://checkthat.gitlab.io/clef2025/task4/

### Collection set
The metadata of CORD-19 papers are provided as a pickle file with the following columns:

> cord_uid <TAB> title <TAB> abstract <TAB> authors <TAB> journal <TAB> [...] (17 metadata columns in total)

**Example:**
See Subtask4b examples at: https://checkthat.gitlab.io/clef2025/task4/

## Output Data Format

The output must be a TSV format with two columns: 
> post_id <TAB> preds

where the "preds" column contains an array of the top5 predicted cord_uids (in descending order: [pred1, pred2, pred3, pred4, pred5], where pred1 is the cord_uid of the highest ranked publication). Please use the provided notebook which contains code to generate the submission file in the correct format. The notebook is available here: https://gitlab.com/checkthat_lab/clef2025-checkthat-lab/-/tree/main/task4/subtask_4b (see "getting_started_subtask4b.ipynb")

## Evaluation Metrics

This task is evaluated as a retrieval task. We will use the Mean Reciprocal Rank (MRR@5) to evaluate it. 

## Baselines & Scorers

A "Getting started" jupyter notebook can be found in this repository. Participants can download it and use it to get started quickly. The notebook contains:

1. Code to upload data, including:
   * code to upload the collection set (CORD-19 academic papers' metadata)
   * code to upload the query set (tweets with implicit references to CORD-19 papers)
2. Code to run a baseline retrieval model (BM25)
3. Code to evaluate the baseline model using the MRR score

## Experimental Implementations

This repository contains several experimental approaches to improve the scientific claim source retrieval task. The experiments are implemented in Jupyter notebooks located in the `experiments/` directory:

### 1. Bi-Encoder Approach (`NLP_Biencoder.ipynb`)
- Implements a dual-encoder architecture using Sentence-BERT
- Fine-tunes the model on the training data to learn tweet-paper relevance
- Uses cosine similarity for retrieval

### 2. Query Expansion (`NLP_QueryExpansion.ipynb`)
- Enhances tweet queries by expanding them with relevant terms
- Uses techniques to identify and add key scientific terms
- Improves retrieval by capturing implicit scientific concepts

### 3. Hybrid Approach (`NLP_Hybrid.ipynb`)
- Combines multiple retrieval methods for improved results
- Integrates lexical and semantic search approaches
- Uses weighted combination of different retrieval scores

### 4. Multi-Hop Fusion (`NLP_MultiHopFusion.ipynb`)
- Implements a multi-step retrieval process
- Uses intermediate results to refine the final retrieval
- Aims to capture complex relationships between tweets and papers

### Hyperparameter Tuning
Three notebooks are dedicated to hyperparameter optimization:
- `NLP_HyperparameterTuning_1.ipynb`
- `NLP_HyperparameterTuning_2.ipynb`
- `NLP_HyperparameterTuning_3.ipynb`

These experiments explore different parameter configurations to optimize model performance.

### Results
The experimental results are stored in:
- `predictions.tsv`: Initial predictions
- `predictions_fromfinetunedmodel.tsv`: Predictions from the fine-tuned model
- `hyperparam_results_1.csv`: Results from hyperparameter tuning

## Submission

Please find the submission details on the Codalab page of the task: https://codalab.lisn.upsaclay.fr/competitions/22359

## Related Work

Information regarding the annotation guidelines can be found in the following document: https://github.com/AI-4-Sci/ReferenceDisambiguation/blob/main/annotation_protocol_examples.pdf 


## Contact
Please contact salim.hafid@sciencespo.fr.

## Credits
Please find it on the task website: https://checkthat.gitlab.io/clef2025/task4/
