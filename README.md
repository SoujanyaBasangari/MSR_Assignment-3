# Baseline
## Metadata
A reproduction of following paper as part of MSR course at MSR course 2020/21 at UniKo, CS department, SoftLang Team

AIMMX: Artificial Intelligence Model Metadata Extractor

DBLP: https://dblp.org/rec/conf/msr/TsayBHSM20.html?view=bibtex

[Paper at the Mining Software Repositories Conference 2020 describing AIMMX](http://www.jsntsay.com/publications/tsay-msr2020.pdf)

The paper presents a library AIMMX which extracts the AI model metadata from software repositories.The input data source is Github. 

## Requirements

Runs on Python3 environment with dependencies as mentined in `requirements.txt`

Required software can be installed by executing code

## Process

To avoid 'Access Denied' errors locally, We have used colab to execute code (https://colab.research.google.com/notebooks/intro.ipynb)

* File > Upload notebook > open aimmx.ipynb 

* To upload all files which are in Files folder,execute command mentioned in first cell

* Go to examples section at end of notebook. Generate GitHub API Key using below steps and then give a github repository link as input.

     * On [GitHub](https://github.com/), click your profile picture in the top-right

     * [Settings > Developer Settings > Personal access tokens > "Generate new token"](https://github.com/settings/tokens)

* Select runtime in menu > Run all (all required installations are done with pip install commands)

* Validate the output metadata with Readme file of corresponding input software repository link.Model name, dataset ,references , framework and domain 
can be verified with corresponding paper

## Data

* Provide a AI related model github repository link as input to AIMMX library. Example repository links are provided in notebook(end of AIMMX.ipynb).AIMMX library process the 
code and refereces of input github repository link and then fetch the metadata of AI model(model name, domain, framework, dataset and references used).

* Dataset used for extracting metadata information are uploaded in Data folder

* AIMMX library code is uploaded in Process folder

* Evaluation results and analysis on evaluation dataset is uploaded in Evaluation folder. Sample output can be checked in evaluation.xlsx


## Delta

## Process Delta

* We have taken complete code from original paper. We have faced 'access denied' and 'installation problems' while executing code locally.We tried to change permission settings 
and used different python IDE's to execute code locally.But still we faced issues.

* To avoid this errors and run code flexibly, we modified the structure of code and merged into a single file(aimmx.ipynb) after understanding the whole process. The merged file 
consists of process of five extractors and util classes.

* We also tried to change the way of extracting references using arxivID, as we still got the same output as original paper we used same logic for extraction.

## Data Delta

* The dataset used for evaluation in paper is very huge(7998 repositories). We have collected dataset of 24 AI model repositories for evaluation and analysis with respect to 
software repositories.We have five different extractors in AIMMX model,sample output can be seen in AIMMX file. 

* Output has been manually evaluated by examining corresponding  github repository code and related papers.

* Evaluation results are almost similiar to the results of original paper, except reference extraction.We got less accuracy for reference extraction compared to the original 
paper.

# Experiment

## Threat 
* There is a risk of subjectivity in the evaluation of incorrect or missing data since some of the evaluations depended on the domain expertise of one of the researchers to establish ground truth of extracted metadata.
* The evaluation dataset contains erroneous repositories that do not contain AI models, according to the explanatory analysis provided in the paper.This is due to lack of filtering AI models.
* While implementing extractors, choosing models from ModelZoos, Arxiv, and SoTA may have introduced bias towards research code rather than enterprise or application code.

## Traces
From paper itself- Internal validity (section 5 –threats to validity)
* “There is chance of subjectivity in the evaluation of incorrect or missing metadata properties. Also, a single researcher perform the analysis whereas using multiple researchers and measuring agreement would have been robust.”
* “Future work should implement a classifier to identify whether a repository contains AI model information”
*  “Similiarly our selection of models from arXiv and soTa may have introduced bias towards research code rather than enterprise or application code.We attempt to mitigate this by including model zoos in dataset”

## Theory
* Researchers with domain expertise are needed to determine the ground truth of extracted metadata. As a consequence, there's a risk that missing and incorrect metadata properties would be subjective. The domain knowledge of one researcher can vary from that of another researcher.To address this threat we are generating the metadata ground truth by mining paperswithcode website.
* As mentioned in the paper, erroneous repositories are being added to the library due to a lack of suitable filtering methods, and there is a risk of incorrect metadata properties being extracted, which may or may not be related to AI. To address this threat and generate more meaningful results, we'll introduce a method to decide if a repository is connected to an AI model or not.
* While implementing the extractors authors have mentioned that they have used modelzoos,sota and arxiv. They have mentioned there might be chance of bias towards research code because of arxiv and soTa. As we are already using the implemented extractors we are trying to check whether any bias exist in extraction of AIMMX. In order to verify that we are fetching metadata using AI repositories which are not part of any repositories(modelzoos,arXiv, soTa) used in paper.

## Feasibility
* As the course is about mining software repositories, we tried using beautifulsoup and the selenium tools to extract metadata from the SoTA repository (paperwithcode web repository) in order to establish ground truth for evaluations. As a whole, we believe our experiment is suitable in the course.

## Implementation
### Threat 1
* We have tried to automate the work which was done manually in the original paper which is creating dataset for evaluation. For doing the same we have used scraping techniques such as Beautifulsoup. 
* We have extracted project metadata information from SOTA repository and generated a sample dataset of nearly 270 projects. In SOTA repositories are segregated according to the topic name. Hence, we tried to fetch all the links in each topic and passed through our function to fetch the metadata information.  
* We are able to fetch Title of project, domain name, datasets used for projects, GitHub link of project and information is saved in a csv file.
* We tried to create a small dataset as it was consuming more time to fetch data for just few links.
* We also attempted to extract metadata from model zoos website  but since the majority of the pages are written entirely in javascript, we were unable to do so with beautifulsoup. We need deeper understanding of selenium tools

### Threat 2
* In order to determine whether repository is AI related or not. We have implemented a method that fetches the tags of repositories and check whether tags are related to AI model or not. As suggested in paper we tried with ML classifier but we discovered that with this method, we can classify with 60% accuracy.And for ML classifier we are not able to generated proper features.

![image](https://user-images.githubusercontent.com/65566187/111851126-7200be00-8912-11eb-892b-a76b1b7f0b08.png)

   * If tags are empty we cannot determine whether repository is AI related or not so we are fetching metadata
   * If tags are not empty and not matching with the list which we created then we are concluding it is AI related
   * If atleast one tag matches then we are fetching metadata

![image](https://user-images.githubusercontent.com/65566187/111851160-88a71500-8912-11eb-97e9-68bdc819c389.png)

### Threat 3
* In order to check for bias in metadata extraction in AIMMX. We've hand-picked a few github repositories that aren't part of modelzoos, arXIV, or paperwithcode and tried to extract metadata.

## Results
*  We are able to generate dataset from paperswithcode website which entirely have AI related models. The generated dataset can be found under data folder.We cross validated repository links in generated dataset and original dataset. Found that SoTa links in original dataset are almost similar with generated dataset.
   ![image](https://user-images.githubusercontent.com/65566187/111871631-803ef080-898b-11eb-8f4a-ca877e4d1acb.png)
   
*  We are able to identify whether repository have AI model or not if repository have tags/ topics mentioned. As some of the repositories don’t have tags/topics to determine whether it is AI related tag or not. We can’t rely completely on this method for classifying. 
*  We haven’t noticed any bias while trying to fetch metadata from other repositories (not part of modelzoos, arXiv, soTa) 

