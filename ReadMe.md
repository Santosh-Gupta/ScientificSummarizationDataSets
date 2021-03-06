Here are several datasets for Scientific Summarization. All the datasets listed below have all digits and special characters filtered out, since I am more 
focused on conceptual summarization rather than factual summarization. 

In addition to the datasets, I have set up preprocessing and training setups using a few popular summarization architectures, in Google Colab notebooks. 

## UPDATE 10-17-19. BertSum model released. 

I have released a checkpoint for the BertSum mode. The model was trained on a batch size of 1024 for 5000 steps, and then a batch size of 4096 for 25000 steps. Please see the BerSum section below. 

## Motivation

Most scientific summarization datasets are from the biomedical domain, but I am currently focuced on summarization of CS concepts, so I needed to make new
datasets for this. I have decided to share the datasets I have made along the way. 

Also, most scientific summarization datasets contain title/abstract pairs, where as I am more interested in summarizing each section of a research paper. 
Thanks to a novel method discovered by Alexios Gidiotis, Grigorios Tsoumakas [https://arxiv.org/abs/1905.07695] , I was able to create datesets for this type of summarization. 

<p align="center">
  <img src="https://i.imgur.com/QYDNT98.png">
</p>


------


## Datasets 

### Title/Abstracts from the Semantic Scholar Corpus

This dataset contains title/abstract pairs from the Semantic Scholar Corpus [  https://api.semanticscholar.org/corpus/ ]. I attempted to filter out any papers 
in the biomedical domain, since title/abstract datasets for the biomedical domain already exists. Though many papers from that domain are still in the dataset, it contains papers from a variety of fields. 

The dataset contains 5.8 million datapoints. The dataset is availible in two forms. 

This is a zip file containing 12 parquet files; it's ~2.5 gb zipped, ~6 gigs unzipped

https://drive.google.com/open?id=1WEdf-_au3vg2EzmWhawmW9xsYaHAE7iV

This is the sqlite database version, 1 file; it's 2.5 gb zipped, 7.5 gb unzipped

https://drive.google.com/open?id=1IhIaBD98BEseteAUi1S_f_SfIaUI8V4D

### Title/Abstracts from ArXiv

This dataset contains title/abstract pairs of every paper on ArXiv, from it's start in 1991 to July 5th 2019. The dataset contains ~10k datapoints from 
quantitive finance, ~26k datapoints from quantitative biology, ~417k datapoints from math, ~1.57 million datapoints from physics, and ~221k datapoints from CS. 
In addition to all the ArXiv categorites, I made a dataset for machine learning papers in ArXiv, so papers from cs.[CV|CL|LG|AI|NE]/stat.ML; this dataset 
contains ~90k papers.

The files are in gzipped parquet format, and are located here

https://drive.google.com/open?id=1CB7ecqdOD4-mD4_Ti-7VVCJzCzUKdeBi

### Paper Section Summaries Using Structured Abstracts. 

The following datasets follow a similar methodology described in Structured Summarization of Academic Publications by Alexios Gidiotis, Grigorios Tsoumakas 
[ https://arxiv.org/abs/1905.07695 ].

Certain papers have abstracts in a structured format; ie a paper abstract may contain a seperate section for the background, methods, results, conclusion, etc. 
Gidiotis/Tsoumakas paired these sections with their corresponding sections in the paper. 

I created two datasets with a similar methodology. The differences in our method are listed below:

-Gidiotis/Tsoumakas used 712,911 papers from PubMed Central. I used ~1.1 million papers from the Sematic Scholar corpus in one dataset, and 3944 papers from 
ArXiv in the other dataset. 

-Gidiotis/Tsoumakas was able to use the tags in the papers' XML to pair the paper sections with the abstract sections. I used AllenAI's Science Parse 
[https://github.com/allenai/science-parse] to split each paper into it's individual sections, and then used the section headers to locate the paper section for 
each abstract section. 

-Gidiotis/Tsoumakas grouped tags containing 'experimental', 'experiments', 'experiment' with tags containing 'results', 'result', 'results'. From my own analysis
of the data, sections with tags/headers containing 'experimental', 'experiments', 'experiment' corresponded to papers containing the tags/headers 
'methods', ' methods', 'method', 'techniques', and 'methodology', so that's how I grouped them. 

-I removed all digits and special characters from each section, since I am more focused on conceptual summarization rather than factial summarization
(I don't have enough cloud space to host the unfiltered datasets)

The ArXiv sectional summarization dataset contains 3944 papers and 6229 total datapoints, in gzipped parquet files. It is available here: 

https://drive.google.com/open?id=1cg_7XMbWYYNOE5DSw2d1oHOCsJkL-InO

The Semantic Scholar sectional summarization dataset contains ~2.3 million data points from ~1.1 million papers, in gzipped parquet files. It is availible here:

https://drive.google.com/open?id=1AH3HEDDs08e-xVRLjAev7K902R0eBrcl

While the Semantic Scholar has papers from a variety of domains, this dataset contains ~99% biomedical papers from my analysis. This likely due to 
two reasions: 1) There are more papers in the biomedical domain than any other domain 2) The Biomedical domain is more likely to have papers which use
the structured abstract format. 

### Access Files Using Pandas 

For the parquet files, you can open them simply using pandas. No need to unzip the gzipped files. 

```
import pandas as pd
df = pd.read_parquet( file.parquet.gz )
```

If there are any issues, you may need to install and use fastparquet. 

```
!pip install fastparquet
df = pd.read_parquet( file.parquet.gz , engine = 'fastparquet') 
```

------


## Preprocessing and Training Setup 

Sometimes preprocessing the data and setting up the training on new summarization data can be tricky. For convenience, I have setup the preproccessing and training for the Pointer-Generator architecture, Bert Extractive (BertSum) architecture, and Transformer architecture using Tensor2Tensor. Each of these needed alterations from the original repos, since the data is formatted differently than the CNN/DailyMail datasets they used. 

In addition, the forked BertSum repo has been altered to use SciBert [ https://github.com/allenai/scibert ] as its starting weights. Allen AI's SciBert has been trained on 1.14 million research papers (18% in the computer science domain, 82% in the biomedical domain), so I felt it is the best set of starting weights for this project. The forked repo has added possible args for new configuration files, pretrained models, and vocab files in order to use the SciBert pretrained weight. These args can also be used for any set of pretrained weights, vocabs, and configurations. At this time, I would also like to give a shoutout to the BioBert pre-trained model [ https://github.com/dmis-lab/biobert ], which I had the pleasure of working with in my previous project. 

The following is a paper for each architecture, original Github repos, my forked versions of those repos, and Colab Notebooks that have the preprocessing/training setup. 

### Pointer Generator 

Paper: https://arxiv.org/abs/1704.04368

Orignal Repos: https://github.com/abisee/pointer-generator https://github.com/abisee/cnn-dailymail https://github.com/f0k/pointer-generator.git

Forked Repo: https://github.com/Santosh-Gupta/cnn-dailymail

Colab Notebook: https://colab.research.google.com/drive/14-hIiDmUE_qmVK0UHVTjyluHoM1yVKnE

### Bert Extractive, using BertSum / SciBert

Paper: https://arxiv.org/abs/1903.10318

Original Repo: https://github.com/nlpyang/BertSum

Forked Repo: https://github.com/Santosh-Gupta/BertSum.git

Colab Notebook: https://colab.research.google.com/drive/1IEHBsryjAjddS0jv7oJOi25_TxjVfA4F

UPDATE 10-17-19. I have released a checkpoint for the BertSum mode. 

CheckPoint at 30,000 training steps: https://drive.google.com/open?id=1-3ftVOOM5HnmX85CztQ8TBWdMOr1WDQp
To use, set the `-train_from` arg to the checkpoint. 


### Transformer Abstractive, using Tensor2Tensor

Papers: https://arxiv.org/abs/1801.10198 https://arxiv.org/abs/1803.07416

Recommended Paper: https://arxiv.org/abs/1906.00138

Original Repo: https://github.com/tensorflow/tensor2tensor

Forked Repo: https://github.com/Santosh-Gupta/tensor2tensor

Colab Notebook: https://colab.research.google.com/drive/1JEfZ2cCJc8Dz_LQMS9_rGgtMgecfXJDG

If you have any questions about running any of the training, please open an issue and I'll get to it as soon as I can. I hope people can develop some effective scientific section summarizars. 

### Heads up

The tokenization steps of the preprocessing produces tokenized files which take up a lot more space than the orignal non-tokenized files. Those files are eventually converted to binary files which take up a lot less space. 

For example, preprocessing ~2.3 millin datapoints for BertSum took about ~340 GB at the peak. The data was eventually converted to binary files which took up ~13 GBs.

## Released Model

For BertSum, I have released a model, which has been trained for 30,000 steps on this training data. The first 5000 steps were trained on a batch size of 1024, and the rest were trained on a batch size of 4096. The Google Drive link is below. 

https://drive.google.com/open?id=1-3ftVOOM5HnmX85CztQ8TBWdMOr1WDQp

To use, set the `-train_from` arg to the checkpoint in either the original BertSum code,

https://github.com/nlpyang/BertSum

Or using my fork. 

https://github.com/Santosh-Gupta/BertSum

If it helps, this is the Colab notebook I used to train over the data
https://colab.research.google.com/drive/1Ujdqe2o7gBHfz1q0iNEIhQMZMLM2qi_b

## Want to be involved?

I am very open to collaborations. Feel free to send me an email at Research2Vec@gmail.com

---------------------------------------------

If citing this work, this is a preferred citation style:

    @article{
    Santosh-Gupta/ScientificSummarizationDataSets,
    author = {Gupta, Santosh},
    title = {Santosh-Gupta/ScientificSummarizationDataSets},
    year = {2019},
    publisher = {Self published by Santosh Gupta},
    howpublished = {\url{https://github.com/Santosh-Gupta/ScientificSummarizationDataSets}},
    }


