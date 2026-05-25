---
title: "Bioinformatik 2 - SoSe 2026"
layout: textlay
excerpt: "bioinfo2"
sitemap: false
permalink: /teaching/bioinfo2
---

## Bioinfo 2 : Machine learning approaches for TFBS prediction
 
MoBi 6.FS - SoSe 2026

Carl Herrmann - IPMB & BioQuant
 
 ----

### Introduction

The goal of these sessions is to investigate how we can **improve the prediction of binding sites for transcription factors** by using additional information (epigenetic signals, sequence conservation, chromatin accessibility), besides sequence only informations based on using a sequence profile (PWM) for TFBS prediction.

This additional information could be for example histone modification datasets (ChIP-seq), information about chromatin opening given e.g. by DNAse hypersensitivity assays, information about DNA methylation, ...

As a gold standard to determine whether a predicted TFBS is a true or false hit, we will consider ChIP-seq datasets for the transcription factor of interest: every predicted TFBS overlapping with a ChIP peak will be considered a true positive, while those outside ChIP peaks will be false positives.

In the following, we will
* Predict potential binding sites using the PWM alone;
* Evaluate the rate of false positives/true positives using as a control ChIP-seq datasets for the TF of interest;
* Determine the signal of the different additional features around true/false binding sites, to see if there is a difference
* Build a model incorporating additional features using logistic regression
* Validate the model using independent datasets.
* Perform variable selection to obtain the simplest, yet accurate model.


#### Datasets: 
for each TF, there is an R rds object available containing a data frame: each line corresponds to a potential TFBS site on chromosome 21, predicted using a position weight matrix; each column corresponds to a feature; the numerical entries represent the scoring of the potential TFBS site with the corresponding feature.
* **accessibility** : DNAse I hypersensitivity data
* **DNAme** : DNA methylation data
* **phylop** : phylogenetic conservation of the bases
* **H3kxxx**: corresponding histone mark around the site
* **LLR** : log-likelihood ratio (=score) of the predicted site (based only on sequence information!)

In addition, **the last columns (tfbs) indicates if this is a true-positive** (i.e. if the predicted site overlaps with a ChIP-seq peak) or not (i.e. no overlap). This is the column we want to predict!

In order to use the file, download the file onto your computer (e.g. `CTCF.rds`) and load it as:

```r
data = readRDS('/path/to/your/file/CTCF.rds')
head(data)
```
Here are the links to the specific datasets; select one TF and click on the link to download the .rds file. The first file contains the predicted TFBS the second one the true ChIP-seq peaks.

* [[CTCF TFBS]](https://www.dropbox.com/s/wbhhrqbuj8mmpk6/CTCF.rds?dl=1); [[CTCF ChIP-seq peaks]](https://www.dropbox.com/scl/fi/62lz0uvjn8msjodsuk01g/CTCF_peaks.rds?rlkey=zauqu2bmimg5ywwutqcpela4k&dl=1)

* [[FOSL2 TFBS]](https://www.dropbox.com/scl/fi/9fdp90fyg5xtuytq8ki48/FOSL2.rds?dl=1); [[FOSL2 ChIP-seq peaks]](https://www.dropbox.com/scl/fi/htyeyym79xs1m0vg593f0/FOSL2_peaks.rds?rlkey=9mwr967a6cem40k5twltvjvde&dl=1)

----- 

### 1. Evaluate the Precision/Recall based on sequence information

**Goal:** We want to evalute the baseline performance of a prediction model which is only based on using a position-weight matrix (pwm), hence is only based on sequence information.

We have scanned the sequence of the human chromosome 21 with a number of PWM for some transcription factors. The output files for each TF contain one line for each predicted TFBS, together with the log-likelihood score (column LLR) of the match, and whether the predicted TFBS can be considered as a true-positive (i.e. overlaps with a ChIP-seq peak for this TF) or a false-positive (i.e does NOT overlap with a ChIP-seq peak.) (column tfbs)

Hence, we have two sets of TFBS:
1. one **positive set** which **intersects true ChIP-seq peaks** (with a 1 in the column tfbs)
2. one **negative set** which does not (with a 0 in the column tfbs)

**Goal:**
* by filtering of the LLR column, select all TFBS with a LLR above a certain threshold (try 50,55,...) 
* based on the tfbs column, compute the precision
* plot the precision curve as a function of the LLR cutoff(it should contain at least 6 points, i.e. 6 different thresholds on the LLR value!).
* plot the ROC-curve (i.e. 1-specificity vs. sensitivity) (you can use the `PRROC` package, check the [vignette here](https://cran.r-project.org/web/packages/PRROC/vignettes/PRROC.pdf))

> **Stop and think!**
> * What is the performance of the sequence based prediction? Does the curve have the expected behavior?
> * Compute the recall, using the number of ChIP-seq peaks as the ground truth
> * Plot precision and recall on the same plot
> * Compare the PR plot with the ROC plot; can you explain the difference?


---- 
### 2. Scoring TFBS with additional features

We will now explore the additional genomic features for all the potential TFBS; in particular, we will look for correlations between the features.

#### Boxplot

We will explore the difference between positive and negative sites by looking at boxplots for each feature for positive/negative tfbs
make a boxplot showing the distribution of the signal between positive and negative sites for each feature run a Wilcoxon test to check for significant differences.

#### Correlation structure
Compute the correlation between the features using the `cor` function in R, and plot the correlation matrix as a heatmap

```r
library(pheatmap)
cor.ft = cor(data[,-16],method="spearman")
diag(cor.ft) = NA
pheatmap(cor.ft)
```

> **Stop and think**\
> Check the interpretation of the different marks [here](https://www.abcam.com/en-us/technical-resources/guides/epigenetics-guide/histone-modifications).
> Which features are most correlated to each other? Does this make sense?

We need to keep in mind the correlation between the variables, as they might impact the results of the regression analysis later. Correlated variables are usually harmfull in regression approaches, as they impact the accuray of the prediction for the regression coefficients of the correlated variables. One solution would be to use a ridge regression or a LASSO method from the glmnet package.

---- 
### 3. Building a logistic regression model

We now come to the core of the machine learning analysis, and train a logistic regression to distinguish optimally the positive from the negative TFBS predictions. This will reveal which variables are most suited to discriminate true from false positives. We will use the glm function in R, using the **binomial model**.

#### First fit of model

*Training the model*

We will now apply the glm function in R, assuming that our binary output variable is distributed according to a binomial distribution with parameter p. All available variables will be included into the model.
Run the glm function by specifying the binomial distribution as family:

```r
lrfit <- glm(tfbs ~ ., data=data,family="binomial")
summary(lrfit)
```

> **Stop and think**\
> Check the output of the `summary` function
> * what are the most relevant variables ?
> * what does the AIC represent ?

*Evaluate the performance*

Now that we have a model, we can apply it to test how good it performs, by using the model as a predictor :

```r
pred = predict(lrfit,newdata=data,type="response")
```

This yields the **predicted probabilities**.

1. Make a boxplot showing the prediction probabilities for the real (tfbs=1) vs. false (tfbs=0) sites. Do the predictions make sense?
2. Set a prediction threshold (e.g. 0.1), make a contingency matrix (tfbs 0/1 vs. above/below prediction threshold) and compute the accuracy = (TP+TN)/(TP+TN+FP+FN); repeat this at various thresholds!
3. Using the `PRROC` package, compute the ROC and PR area under the curve (AUC). Plot the ROC and PR curves. Did we improve?
4. How does the cross-entropy look like?

> **Stop and think hard!** \
> What would be the value of the accuracy if you would predict all sites to be negative?

#### Cross validation to evaluate the performance

Of course we have cheated here: we are evaluating the performance of the model on the data that was used to train the model, thus biasing the evaluation.... The proper way to evaluate the performance of a machine-learning model is using a cross-validation : splitting the data into a training and testing set, learning the model on the training set and applying it to the test set, thus evaluating the performance on an independent dataset.

*Creating a partition*

We will create a training dataset with 75% of the data, and keep the remaining 25% for the testing dataset; we need to make sure to sample 75% of the positive and the negative samples!

```r
i.pos = which(data$tfbs==1)
i.pos.train = sample(i.pos,floor(0.75*length(i.pos)))
i.neg = which(data$tfbs==0)
i.neg.train = sample(i.neg,floor(0.75*length(i.neg)))
i.train = c(i.pos.train,i.neg.train)
train.set = data[i.train,]
test.set = data[-i.train,]
```

We have created a balanced dataset, meaning that we have sampled proportionaly from each class. To do this, you can also use the `caret` package in R, which has a function `createDataPartition` which allows to do this. Check [this web page](https://topepo.github.io/caret/data-splitting.html) for explanations on this function!

*Train the model*

Run the glm function to fit the model, while evaluating the performance using the cross-validations

```r
lrFit = glm(tfbs~ .,data=train.set,family="binomial")
````

*Applying the model*

Remember that we had defined a training and a test set? Now that we have an optimal model, we can apply it to the left-out test dataset...

1. run the following function
```r 
pred <- predict(lrFit,newdata=test.set,type="response")
```
2. Determine the precision here (use one particular threshold!).

>Did we improve compared to the sequence-only performance?

---- 

### 4. Variable selection

We have now a good model, which includes all variables. We can now try to simplify the model by reducing it, removing "uninteresting" variables that do not contribute to the performance of the model ...


*Subset selection*

We can apply a built in function step, which recursively eliminates variables
```r
lrfit <- glm(tfbs ~ ., data=train.set,family="binomial")
lr.red <- step(lrfit)
```

> **Stop and think**
> 1. What is happening here? What criteria is used to eliminate variables?
> 2. Check the final model stored in lr.red: what variables does it contain? Is this expected?
> 3. Apply this reduced model to the left out `test.set`
> 4. Did the performance in terms of precision or recall get worse?

#### Shrinkage methods

The `glmnet` package implements ridge regression and LASSO shrinkage methods. If it is not installed, install the package with

```r
install.packages('glmnet')
```

Check the [tutorial for this package](https://glmnet.stanford.edu/articles/glmnet.html)! 

*Ridge regression*

Start by performing a ridge regression using the following code:

```r
library(glmnet)
var = data[,-16]
out=as.integer(data[,16])
fit.ridge = glmnet(data.matrix(var),out,alpha=0,family="binomial")
```

Now you can graphically plot the coefficients, as the regularization parameter lambda is varied:

```r
plot(fit.ridge,label=TRUE,lwd=2)
````

The number on the lines corespond to the column number of the corresponding variable.
Can you interpret the plot? Is it consistent with the previous stepwise variable selection?

*LASSO*

LASSO implements a different regularization compared to ridge regression (i.e. L1 regularization), which has the effect to set a number of variables to zero, hence leading to a selection of relevant variables:

```r
library(glmnet)
var = data[,-16]
out=as.integer(data[,16])
fit.lasso = glmnet(data.matrix(var),out,alpha=1,family="binomial")
````

Now you can graphically plot the coefficients, as the regularization parameter lambda is varied:

```r
plot(fit.lasso,label=TRUE,lwd=2)
```

Is the selection of variables consistent with the backward procedure?

---- 

### 5. Random Forest

There are many other machine-learning approaches for classification tasks. One popular methd is **Random forest**, which is an ensemble method based on classification trees. You can install the R packages `randomForest`

```r
install.packages('randomForest')
```

#### Run the random forest

Filter the initial dataset using an appropriate cutoff on the LLR for the motif, the run the random forest:

```r
library(randomForest)
rf = randomForest(tfbs ~., data = filtered_data, ntree=500)
print(rf)
```

> **Stop and think**
>* what is the number of variables at each split? 
>* What is the OOB error? 
>* What is the class error?

You might notice some weird results in the class error. Why is that? Create a dataset which is more balanced between positives and negatives, and run the random forest again!

#### Variable importance

Using randomForest, we can evaluate the importance of each variable for the classification. This is estimated using the Gini purity index

```r
importance(rf)
varImpPlot(rf)
```
----

## Take home

* **Precision-recall curves** provide more informative evaluation than accuracy alone when dealing with highly imbalanced datasets.
* **Logistic regression** fundamentals: Binomial GLM  transforms multiple continuous genomic features into binding probability predictions.
* **Multicollinearity challenges**: Correlated features (like related histone marks) affect coefficient estimation and interpretation in regression models.
* **AIC-guided model selection**: Stepwise procedures use information criteria to balance model complexity and goodness-of-fit.
* **Regularization methods**: Ridge regression (L2) and LASSO (L1) address overfitting and perform automatic feature selection.
* **Cross-validation importance**: Testing models on independent data prevents performance overestimation and confirms generalizability.
* **Variable importance metrics**: Statistical methods can quantify each feature's contribution to prediction accuracy across different model types.

----

### Suggested Reading:

* Machine-learning for TFBS
Ernst, J., Plasterer, H. L., Simon, I., & Bar-Joseph, Z. (2010). Integrating multiple evidence sources to predict transcription factor binding in the human genome. [Genome Research, 20(4), 526–36](https://pmc.ncbi.nlm.nih.gov/articles/PMC2847756/)

* James, Witten, Hastie, Tibshirani (2013) [An introduction to statistical learning](https://www.statlearning.com)

* James, Witten, Hastie, Tibshirani (2017) [The Elements of Statistical Learning](https://hastie.su.domains/ElemStatLearn/)

 
 
 