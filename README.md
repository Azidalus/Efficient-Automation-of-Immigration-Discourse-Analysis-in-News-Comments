# Efficient-Automation-of-Immigration-Discourse-Analysis-in-News-Comments
*In this repository, you can find a dataset of 11k user comments to immigration-related news articles labeled with 15 immigration-related topics and stances towards immigration (positive, negative, and unclear), as well as Python notebooks with the models used for that task.*

## Table of contents
* [Project background](https://github.com/Azidalus/Efficient-Automation-of-Immigration-Discourse-Analysis-in-News-Comments#Project-background)
* [Data structure](https://github.com/Azidalus/Efficient-Automation-of-Immigration-Discourse-Analysis-in-News-Comments#Data-structure)
* [Methodology](https://github.com/Azidalus/Efficient-Automation-of-Immigration-Discourse-Analysis-in-News-Comments#Methodology)
* [Recommendations](https://github.com/Azidalus/Efficient-Automation-of-Immigration-Discourse-Analysis-in-News-Comments#Recommendations)

## Project background
The HYBRIDS organization aims to explore how people with varying attitudes toward immigration express their thoughts, but requires labeled data to do so.

This project focuses on efficient automated labeling of a dataset requested by HYBRIDS with discussion topics and stances towards immigration (positive, negative, or unclear). To achieve that, several NLP models are applied and a web-application for their further use is created. 

## Data structure
The dataset comprises approximately 11,000 user comments on 67 immigration-related online news articles. Here's an example data row: 

<p align="center">
<img src="https://github.com/user-attachments/assets/d1058b8f-50aa-4786-9642-a431381b6d24" height="240">
</p>

We work only with the “comment” field. However, as a note, “source” is the comment’s id in the tree of comments to an article.

## Methodology

### Approach for topics
For topics, I first used 2 unsupervised topic models, LDA and CorEx, and they both produced not super promising results: only 8-10 interpretable topics out of 18, and even then too generic. That is why a semi-supervised topic model Anchored CorEx was then used. Three independent coders manually labeled a sample of 670 comments with topics (and proactively, stances). In total, we found 16 immigration-related topics. I then extracted useful terms for each topic from the labeled sample and seeded Anchored CorEx to guide it towards these topics. 
(The coding guideline can be found in repository as well)

### Approach for stances
For stances, I explored the use of Anchored CorEx and a conversational language model Llama-3. The first approach proved to be not feasible at all, and the latter model turned out an efficient solution for stance labeling, with accuracy at 69%.
