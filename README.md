# Efficient-Automation-of-Immigration-Discourse-Analysis-in-News-Comments
*In this repository, you can find a dataset of 11k user comments to immigration-related news articles labeled with 15 immigration-related topics and stances towards immigration (positive, negative, and unclear), as well as Python notebooks with the models used for that task.*

### Task
I was tasked with to efficiently label a dataset of 11k user comments to immigration-related news articles with discussion topics and stances towards immigration. To achieve that, I applied several NLP models to automate the task. 

### Approach for topics
For topics, I first used 2 unsupervised topic models, LDA and CorEx, and they both produced not super promising results: only 8-10 interpretable topics out of 18, and even then too generic. That is why a semi-supervised topic model Anchored CorEx was then used. Three independent coders manually labeled a sample of 670 comments with topics (and proactively, stances). In total, we found 16 immigration-related topics. I then extracted useful terms for each topic from the labeled sample and seeded Anchored CorEx to guide it towards these topics. 
(The coding guideline can be found in repository as well)

### Approach for stances
For stances, I explored the use of Anchored CorEx and a conversational language model Llama-3. The first approach proved to be not feasible at all, and the latter model turned out an efficient solution for stance labeling, with accuracy at 69%.
