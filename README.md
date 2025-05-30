# CLEF 2025 Subtask 4b (Scientific Claim Source Retrieval): Team "SourceSniffers"
Winner of [CLEF 2025 Subtask 4b](https://checkthat.gitlab.io/clef2025/task4/)

## Overview 

This project tackles the Scientific Claim Source Retrieval task using three distinct approaches:

- Traditional IR with BM25 [Onbas, Haberl]

- Dense Bi-Encoder Retrieval (NLP Represenation Learning) [Onbas]

- Neural Re-Ranking (used for the final submission) [Haberger, Haberl]

Our primary goal was to retrieve relevant scientific sources for a given claim with high precision. We achieved **1st place** on the competition leaderboard with our final submission.

## Structure

Note that only the code from the neural reranking is currently merged into the master. If you want to have a look at the other implementations, switch to the other branches.
The branch **NLP-RL** contains our code and experiments regarding the representation learning approach. The branch **Traditional-IR** contains our code and experiments regarding the traditional approach (using rank_bm25).

- **data/**: Directory containing the dataset and the predictions from our models.
- **model/**: Directory containing all model checkpoints
- **models/** Directory containing the best model for each experiment (based on validation dataset)
- **basic_data_analysis.ipynb**: Notebook containing our basic analysis of the dataset to get a feeling for the data and which features might be informative.
- **reranking_ensemble.ipynb**: Notebook containing our code and experiments looking into using an ensemble of rerankers.
- **reranking.ipynb**: Notebook containing our code and experiments to optimize the performance of pre-trained rerankers.
- **results.ipynb**: Reads all the data from our reranking experiments, evaluates the performance of each experiment and merges the info into one big dataframe.
- **traditional_approach.ipynb**: Notebook containing some of our code and experiments regarding the traditional approach. This builds on top of the experiments in the Traditional-IR branch. We created a new notebook and some additional experiments since the rank_bm25 library is way to slow if you want to experiment with reranking. Therefore we switched to the bm25s library and did some additional experiments to verify that it works comparably well to our previous approach.

## Setup
This project is fully configured for use with VS Code Dev Containers (or any compatible remote development tool). 

Clone the repo and open in VS Code

Reopen in container when prompted

The environment is defined in the included devcontainer.json:

- Uses Python 3.12
- Includes CUDA support (for GPU acceleration if available)
- Automatically installs all required pip dependencies

This should work for any editor that supports Dev Containers (Neovim, IntelliJ, etc.).

Alternatively, you can do the setup manually. In this case you have install Python 3.12 and CUDA. Afterwards use the requirements.txt file to install the pip-requirements.

## Final Submission Model
- Based on alibaba/gte-reranker-modernbert-base
- Finetuned on the official dataset.
- LambdaLoss
- Hard Negative Mining (num hard negatives=5, range min=3, range max=100, max score=0.95, margin=0.0)
- MRR@10 validation
- Learning Rate with Weight Decay
- Reranks the top-1000 BM25 documents.

→ Devset Performance MRR@5: 0.7725
→ Testset Performance MRR@5 = 0.683 (1st place)



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

## Credits
Please find it on the task website: https://checkthat.gitlab.io/clef2025/task4/
