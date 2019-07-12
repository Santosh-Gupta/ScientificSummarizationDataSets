Here are several datasets for Scientific Summarization. All the datasets listed below have all digits and special characters filtered out, since I am more 
focused on conceptional summarization rather than factual summarization. 

### Motivation

Most scientific summarization datasets are from the biomedical domain, but I am currently focuced on summarization of CS concepts, so I needed to make new
datasets for those. I have decided to share the datasets I have made along the way. 

Also, most scientific summarization datasets contain title/abstract pairs, where as I am more interested in summarization each section of a research paper. 
Thanks to a novel method discovered by Alexios Gidiotis, Grigorios Tsoumakas [https://arxiv.org/abs/1905.07695] , I was able to create datesets for this type of summarization 

------

### Title Abstracts from the Semantic Scholar Corpus

This dataset contains title/abstract pairs from the Semantic Scholar Corpus [  https://api.semanticscholar.org/corpus/ ]. I attempted to filter out any papers 
in the biomedical domain, since title/abstract datasets for the biomedical domain already exists. Though many papers from that domain still exist, the dataset 
contains papers from a variety of fields. 

The dataset contains 5861443 datapoints. The dataset is availible in two forms. 

This is a zip file containing 12 parquet files
https://drive.google.com/open?id=1WEdf-_au3vg2EzmWhawmW9xsYaHAE7iV
it's ~2.5 gb zipped, ~6 gigs unzipped

This is the sqlite database version, 1 file
https://drive.google.com/open?id=1IhIaBD98BEseteAUi1S_f_SfIaUI8V4D
it's 2.5 gb zipped, 7.5 gb unzipped

### Title Abstracts from ArXiv

This dataset contains title/abstract pairs of every paper on ArXiv, from it's start in 1991 to July 5th 2019. The dataset contains ~10k datapoints from 
quantitive finance, ~26k datapoints from quantitative biology, ~417k datapoints from math, ~1.57 million datapoints from physics, and ~221k datapoints from CS. 
In addition to all the ArXiv categorites, I made a dataset for machine learning papers in ArXiv, so papers from cs.[CV|CL|LG|AI|NE]/stat.ML; this dataset 
contains ~90k papers.

The files are in gzipped parquet format, and are located here

https://drive.google.com/open?id=1cg_7XMbWYYNOE5DSw2d1oHOCsJkL-InO

### Paper Section Summaries Using Structured Abstracts. 

The following datasets follow a similar methodolgy described in Structured Summarization of Academic Publications by Alexios Gidiotis, Grigorios Tsoumakas 
[ https://arxiv.org/abs/1905.07695 ].

Certain papers have abstracts in a structured format; ie a paper abstract may contain a seperate section for the background, methods, results, conclusion, etc. 
Gidiotis/Tsoumakas paired these sections with their corresponding sections in the paper. 

I created two datasets with a similar methodology. The differences in our method are listed below:

-Gidiotis/Tsoumakas used 712,911 PubMed Central. I used ~4.3 million papers from the Sematic Scholar corpus in one dataset, and 3944 papers from 
ArXiv in the other dataset. 

-Gidiotis/Tsoumakas was able to use the tags in the papers XML to pair the paper sections with the abstract sections. I used AllenAI's Science Parse 
[https://github.com/allenai/science-parse] to split each paper into it's individual sections, and then used the section headers to locate the paper section for 
each abstract section. 

-Gidiotis/Tsoumakas grouped tags containing 'experimental', 'experiments', 'experiment' with tags containing 'results', 'result', 'results'. From my own analysis
of the data, sections with tags/headers containing 'experimental', 'experiments', 'experiment' corresponded to papers containing the tags/headers 
'methods', ' methods', 'method', 'techniques', and 'methodology', so that's how I grouped them. 

The ArXiv sectional summarization dataset contains 3944 papers and 6229 total datapoints, , in gzipped parquet files. It is available here: 

https://drive.google.com/open?id=1cg_7XMbWYYNOE5DSw2d1oHOCsJkL-InO

The Semantic Scholar sectional summarization dataset contains ~11 million data points from ~4.3 million papers, in gzipped parquet files. It is availible here:

https://drive.google.com/open?id=1AH3HEDDs08e-xVRLjAev7K902R0eBrcl

While the Semantic Scholar has papers from a variety of domains, this dataset contains ~99% biomedical papers from my analysis. This likely due to 
two reasions: 1) There are more papers in the biomedical domain than any other domain 2) The Biomedical domain is more likely to have papers which use
the structured abstract format. 

------

Citing
This is a preferred citation style:

@article{Santosh-Gupta/ScientificSummarizationDataSets,
    author = {Gupta, Santosh},
    title = {Santosh-Gupta/ScientificSummarizationDataSets},
    year = {2019},
    publisher = {Self published by Santosh Gupta},
    howpublished = {\url{https://github.com/Santosh-Gupta/ScientificSummarizationDataSets}},
}


