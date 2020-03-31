
# Automated Store Walks:  A Model With Available Data

![](MVIMG_20200325_202944.jpg)

### Background
Store level retail in grocery is easy to oversimplify. Succinctly, you just have to ask:

1. Is the product on the shelf?
2. If yes, look at the next product.
3. If no, put the product on the shelf and move on to the next product.

As real and simple as this process is, it obscures the nuances that actually *get* product onto the sales floor. It doesn't ask whether the product was ordered; it doesn't ask whether the warehouse has run out; it doesn't ask whether the vendor is out of stock or whether they've been in for the day; it doesn't ask whether there are issues related to overnight replenishment - it just asks:  is it on the shelf?

To get an answer to this question, operators have created multiple tools to assist with inventory management. However, no tool generated to date has come as close to answering the question as well or as reliably as simply inspecting the sales floor. This is often time and labor intensive:  investing in a detail walk of every UPC on a sales floor is tedious; and often as a manager you need to anticipate multiple interruptions. Frequently, only a percent of the sales floor can be detail walked. So, as a manager, you have to make a judgment call about how and where to invest the time you have to maximize your return.

### Objective
Bearing this in mind, tools that can effectively point out potential sales floor issues are a great way for a manager or hourly associate to maximize their efficiency. Tools that are frequently in use force us to zero in on big sales and gross profit drivers. Other niche application tools can help us highlight unique shelf issues, such as displays; but there's still no substitute for walking the store in full detail.

As such, in this project I aim to take simple sales and inventory data from BI to create a model which can effectively flag shelf issues.

# Table of Contents

1. [Preprocessing](Preprocessing.ipynb)
2. [Unification and Feature Engineering](Unification%20and%20Feature%20Engineering.ipynb)
3. [Early Modeling](Modeling.ipynb)
4. [EDA and Tweaking Some Features, Final Model Concept](EDA.ipynb)
5. [Conclusion](Conclusion.ipynb)

### Additional Materials
1. [Presentation](Presentation.pdf)

# Results

A review of the data explains that the data itself is not very feature rich. Limited access to systems meant that data from Microsoft's BI would have to be used in a limited capacity to estimate features already found in our (Store Replenisment System). Sales and billing data from BI across different time frames did prove useful, and was used to reverse engineer several set features, but it's worth noting that with access to the more unique features in SRS (last date ordered, perpetual last changed, shelf capacity, etc) the performance of the final model could potentially be further improved.

After joining all the extracted data from BI and verifying it for integrity, the data was found imbalanced. In a set of 967 UPCs, only about 6% presented shelf level issues on the date of survey. While representing a significant store level issue (60+ holes in only about an aisle and a half), the data presented a signficant statistical challenge:  a baseline model that *only* predicted there were no issues would be 94% accurate. In order for the models to be valuable, they would have to be at least 94% accurate themselves.

Initial attempts at modeling did not yield satisfactory results. More complex models often had satisfactory accuracy but lacked sufficient recall - which is to say that, if a manager were to execute the tool, they would still leave a vast majority of shelf issues unchecked.

SMOTE is recruited to upsample the imbalanced set and train an XGBoost classifier on the test set. On training data, the XGBoost classifier is about 95% accurate and has a 97% recall score. On test data, the model retained 95% accuracy but its recall dropped to 50%. In repeated train/test splits, recall scored consistently between 50-66% - meaning a manager could successfully execute the recommendations and capture at least half the shelf issues present.

![](Capture.PNG)

![](Confusion.PNG)
