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
The dataset comprises approximately 11,000 user comments on 75 immigration-related online news articles. Here's an example data row: 

<p align="center">
<img src="https://github.com/user-attachments/assets/d1058b8f-50aa-4786-9642-a431381b6d24" height="240">
</p>

We work only with the `comment` field. However, as a note, `source` is the comment’s id in the tree of comments to an article.

## Methodology

### Approach for topics
We hypothesize unsupervised models will struggle with the data due to its small size (10,982 documents) and topic-homogeneity and aimed to reserve time for potentially more promising semi-supervised models, since we need reliable results for a real-world
application.
After indeed seeing not the best results, manual data coding is performed to get a better idea of the underlying topics in the dataset and obtain ground-truth labels for semi-supervised-models. Finally, a semi-supervised topic model Anchored CorEx is used to identify the learnt topics and possibly discover more unknown topics, evaluate the result, and use it to label the dataset with immigration topics.

For topics, I first used 2 unsupervised topic models, LDA and CorEx, and they both produced not super promising results: only 8-10 interpretable topics out of 18, and even then too generic. That is why a semi-supervised topic model Anchored CorEx was then used. Three independent coders manually labeled a sample of 670 comments with topics (and proactively, stances). In total, we found 16 immigration-related topics. I then extracted useful terms for each topic from the labeled sample and seeded Anchored CorEx to guide it towards these topics. 
(The coding guideline can be found in repository as well)

#### Evaluation 
Since the stakeholders' need is to have the dataset labeled with a sufficient number of specific topics (10-20) rather than to find all possible underlying topics, evaluation revolves around how effectively the model detects the topics of our interest. It is achieved by comparing predicted topics with the ground truth topics from the labeled subset and assessing the model’s performance using Precision and Recall.

### Approach for stances
For stances, few methods are explored: Anchored CorEx and a conversational language model Llama-3. The first approach proved to be not feasible at all, and the latter model turned out an efficient solution for stance labeling.

Because the model can occasionally assign different stances to the same comment, stance prediction is performed three times, resulting in each comment having 3 predicted stances. The final stance for the comment is determined by the most prevalent stance among the three, and if all three stances differ, the final stance is labeled as “-”.

#### Evaluation 
For evaluation, Accuracy of the predicted labels and per-stance Precision and Recall are used.

## Results

### Topics


### Stances
The Llama model has 64% overall Accuracy, with particular success in identifying “neg” and “none” stances. The model detects nearly all anti-immigration comments (92%), although sometimes assigning that stance wrongly. It has some difficulty recognizing “none” stance comments (only 54% are spotted), but on the other hand almost all (88%) such predictions are correct. The positive stance is the most challenging for the model to detect (only 16% of comments are spotted), also with enough faulty predictions. 

If we exclude the comments where the model is uncertain, Accuracy and all metrics for all stances improve even further. 97% of negative comments are recognized, still with occasional mispredictions. “None” stance comments are still a bit of a struggle, only 55% are spotted, though we have a 94% chance that if a comment is predicted to have this stance, it is true. The biggest challenge continues to be the “pos” stance - it is difficult to recognize it, although if the model does see it, there is 80% that it is correct.

## Future improvements
