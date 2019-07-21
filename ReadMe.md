Here are several datasets for Scientific Summarization. All the datasets listed below have all digits and special characters filtered out, since I am more 
focused on conceptual summarization rather than factual summarization. 

Update 7-20-19

I have forked several Github repos to set them up to train on the data, and then created accompanying Colab notebook which shows how to use the repos to process and training on the data. For abstractive summarization, I have selected summarization repos which use the Transformers architecture, and the Pointer-Generator architecture. And for extractive summarization, I have selected a Bert-based architecture. They are all listed at the bottom. 

### Motivation

Most scientific summarization datasets are from the biomedical domain, but I am currently focuced on summarization of CS concepts, so I needed to make new
datasets for this. I have decided to share the datasets I have made along the way. 

Also, most scientific summarization datasets contain title/abstract pairs, where as I am more interested in summarizing each section of a research paper. 
Thanks to a novel method discovered by Alexios Gidiotis, Grigorios Tsoumakas [https://arxiv.org/abs/1905.07695] , I was able to create datesets for this type of summarization. 

<p align="center">
  <img src="https://i.imgur.com/QYDNT98.png">
</p>


------

### Title Abstracts from the Semantic Scholar Corpus

This dataset contains title/abstract pairs from the Semantic Scholar Corpus [  https://api.semanticscholar.org/corpus/ ]. I attempted to filter out any papers 
in the biomedical domain, since title/abstract datasets for the biomedical domain already exists. Though many papers from that domain are still in the dataset, itcontains papers from a variety of fields. 

The dataset contains 5.8 million datapoints. The dataset is availible in two forms. 

This is a zip file containing 12 parquet files; it's ~2.5 gb zipped, ~6 gigs unzipped

https://drive.google.com/open?id=1WEdf-_au3vg2EzmWhawmW9xsYaHAE7iV

This is the sqlite database version, 1 file; it's 2.5 gb zipped, 7.5 gb unzipped

https://drive.google.com/open?id=1IhIaBD98BEseteAUi1S_f_SfIaUI8V4D

### Title Abstracts from ArXiv

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

-Gidiotis/Tsoumakas used 712,911 papers from PubMed Central. I used ~4.3 million papers from the Sematic Scholar corpus in one dataset, and 3944 papers from 
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

The Semantic Scholar sectional summarization dataset contains ~11 million data points from ~4.3 million papers, in gzipped parquet files. It is availible here:

https://drive.google.com/open?id=1AH3HEDDs08e-xVRLjAev7K902R0eBrcl

While the Semantic Scholar has papers from a variety of domains, this dataset contains ~99% biomedical papers from my analysis. This likely due to 
two reasions: 1) There are more papers in the biomedical domain than any other domain 2) The Biomedical domain is more likely to have papers which use
the structured abstract format. 

---------

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

Update 7-20-19

I have forked several Github repos to set them up to train on the data, and then created accompanying Colab notebook which shows how to use the repos to process and training on the data. For abstractive summarization, I have selected summarization repos which use the Transformers architecture, and the Pointer-Generator architecture. And for extractive summarization, I have selected a Bert-based architecture. 

## Pointer Generator 

Paper: https://arxiv.org/abs/1704.04368

Orignal Repos: https://github.com/abisee/pointer-generator https://github.com/abisee/cnn-dailymail https://github.com/f0k/pointer-generator.git

Forked Repo: https://github.com/Santosh-Gupta/cnn-dailymail

Colab Notebook: https://colab.research.google.com/drive/14-hIiDmUE_qmVK0UHVTjyluHoM1yVKnE

## Bert Extractive (BertSum)

Paper: https://arxiv.org/abs/1903.10318

Original Repo: https://github.com/nlpyang/BertSum

Forked Repo: https://github.com/Santosh-Gupta/BertSum.git

Colab Notebook: https://colab.research.google.com/drive/1IEHBsryjAjddS0jv7oJOi25_TxjVfA4F

## Transformer, using Tensor2Tensor

Papers: https://arxiv.org/abs/1801.10198 https://arxiv.org/abs/1803.07416

Recommended Paper: https://arxiv.org/abs/1906.00138

Original Repo: https://github.com/tensorflow/tensor2tensor

Forked Repo: 

Colab Notebook: https://colab.research.google.com/drive/1JEfZ2cCJc8Dz_LQMS9_rGgtMgecfXJDG

If you have any questions about running any of the training, please open an issue and I'll get to it as soon as I can. 

------------------------------------------

If citing this work, this is a preferred citation style:

    @article{
    Santosh-Gupta/ScientificSummarizationDataSets,
    author = {Gupta, Santosh},
    title = {Santosh-Gupta/ScientificSummarizationDataSets},
    year = {2019},
    publisher = {Self published by Santosh Gupta},
    howpublished = {\url{https://github.com/Santosh-Gupta/ScientificSummarizationDataSets}},
    }


