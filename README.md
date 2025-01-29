# ClinicalTrialLLM: Matching Patients to Clinical Trials with Large Language Models

## Introduction
Clinical trials are often hindered by the challenge of patient recruitment. In this work, we introduce ClinicalTrialLLM, a novel end-to-end framework for zero-shot patient-to-trial matching with large language models (LLMs). ClinicalTrialLLM consists of three key components: it first performs filtering of irrelevant clinical trials at scale (ClinicalTrialLLM-Retrieval), then predicts the patient eligibility on a criterion-by-criterion basis (ClinicalTrialLLM-Matching); and finally aggregates criterion-level predictions into trial-level scores for ranking the clinical trials (ClinicalTrialLLM-Ranking). We evaluate ClinicalTrialLLM on three publicly available cohorts of 183 synthetic patients with over 75,000 trial eligibility annotations. ClinicalTrialLLM-Retrieval can efficiently recall over 90% of relevant clinical trials using only less than 6% of the initial clinical trial collection. Furthermore,ClinicalTrialLLM can significantly reduce the screening time by 50% in a real-life clinical trial matching task. Taken together, these results have demonstrated promising opportunities for clinical trial matching with LLMs via the ClinicalTrialLLM framework.

![image](https://github.com/user-attachments/assets/66b01b03-1871-4ccc-be05-10e17e077370)

## Configuration

To run TrialGPT, one needs to first set up the OpenAI API either directly through OpenAI or through Microsoft Azure. Here we use Microsoft Azure because it is compliant with Health Insurance Portability and Accountability Act (HIPAA). Please set the enviroment variables accordingly:

```bash
export OPENAI_ENDPOINT=YOUR_AZURE_OPENAI_ENDPOINT_URL
export OPENAI_API_KEY=YOUR_AZURE_OPENAI_API_KEY
```

The code has been tested with Python 3.9.13 using CentOS Linux release 7.9.2009 (Core). Please install the required Python packages by:

```bash
pip install -r requirements.txt
```

## Datasets

We used the clinical trial information on https://clinicaltrials.gov/. Please download our parsed dataset by:

```bash
wget -O dataset/trial_info.json https://ftp.ncbi.nlm.nih.gov/pub/lu/TrialGPT/trial_info.json
```

Three publicly available datasets are used in the study (please properly cite these datasets if you use them; see details about citations in the bottom):
- The SIGIR 2016 corpus, available at: https://data.csiro.au/collection/csiro:17152
- The TREC Clinical Trials 2021 corpus, available at: https://www.trec-cds.org/2021.html
- The TREC Clinical Trials 2022 corpus, available at: https://www.trec-cds.org/2022.html

The SIGIR dataset is already in `/dataset/`, please download the corpora of TREC CT 2021 and 2022 by:

```bash
wget -O dataset/trec_2021/corpus.jsonl https://ftp.ncbi.nlm.nih.gov/pub/lu/TrialGPT/trec_2021_corpus.jsonl
wget -O dataset/trec_2022/corpus.jsonl https://ftp.ncbi.nlm.nih.gov/pub/lu/TrialGPT/trec_2022_corpus.jsonl
```

## TrialGPT-Retrieval

Given a patient summary and an initial collection of clinical trials, the first step is TrialGPT-Retrieval, which generates a list of keywords for the patient and utilizes a hybrid-fusion retrieval mechanism to get relevant trials (component a in the figure). 



After generating the keywords, one can run the code below for retrieving relevant clinical trials. The retrieved trials will be saved in the `./results/` directory.

## TrialGPT-Matching

After retrieving the candidate clinical trials with TrialGPT-Retrieval, the next step is to use TrialGPT-Matching to perform fine-grained criterion-by-criterion analyses on each patient-trial pair (component b in the figure).
## TrialGPT-Ranking

The final step is to use TrialGPT-Ranking to aggregate the criterion-level predictions into trial-level scores for ranking (component c in the figure).
```



Example output:

```bash
Patient ID: sigir-20142
Clinical trial ranking:
NCT00185120 2.8999999995
NCT02144636 2.8999999995
NCT02608255 2.84999999975
NCT01724996 2.7999999998
...
```

