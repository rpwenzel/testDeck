---
title       : PML Project
subtitle    : Predicting a Participant's Exercise Manner
author      : Bob Wenzel
job         :
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      #
widgets     : [bootstrap, quiz, shiny, interactive]      # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides


---  Predicting the Manner

##         Predicting a Participant's Exercise Manner 
### Introduction
"Human Activity Recognition - HAR has emerged as a key research area in the last years and is gaining increasing attention by the pervasive computing research community" [1].   The task at hand, using their data from their research in "Weight Lifting Exercises" is to predict the manner in which a participant did their exercises.  The manner performed can be one of 5 Classes:
 - (Class A) exactly to specification
 - (Class B) throwing the elbows to the front
 - (Class C) lifting the dumbbell only halfway
 - (Class D) lowering the dumbbell only halfway
 - (Class E) throwing the hips to the front.  
(Class A) corresponds to the specified execution of the exercise and the other 4 classes are related to common mistakes.

---
Data for this project can be found at:
 - For Trainning: https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv
 - For Testing: https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv

Using exploratory analysis and predictive techniques such as Random Forest, we will predict what manner a participant did based upon exploring the data we received.  Ideally the OOB will small, preferably less the 5%. 

### Methods - Data Collection
The training data set consisted of 19622 records with 160 variables, one of which specified the Class (classe). The testing data set consisted of 20 records with 160 variables without the variable Class which is the object of this project to predict.
The data was downloaded from the Coursera Class "Practical Machine Learning" site[2] on 2014-06-14.

---

### Exploratory Analysis
Examining the training raw data, many NA values were observed.  So two transformations were developed to remove the columns with the NA values.  

Six additional columns were removed since they didn't appear to be measurement type variables (X, user_name, new_window, raw_timestamp_part_1, raw_timestamp_part_2 and cvtd_timestamp.  Presuming the test data came from the same population of the training data, the same transformations were performed along with the removal of the six additional columns.  

In both cases the columns were the same except for the last one column.  The training data last column was "classe" and the testing data last column was "problem_id".  We changed it to be "classe" too so randomForest would be able to be run on the training data. 

Once the data was cleansed of the "NA" values, the training data set was divided into a smaller training set (80% randomly chosen rows) and into a new testing data set using the remaining rows from the original training set.

---

### Statistical Modeling
Based on our exploratory analysis, we used the Random Forest method for Classification and Regression to create a model for our data.  We initially used the 80% training data in our randomForest model, but it was taking a very long time. 

 - modFit <- randomForest(training$classe ~.,data = training)

So after 30 minutes, we cancelled the randomForest execution and reran it again but using the smaller 20% testing data set. 

 - modFit <- randomForest(testing$classe ~.,data = testing)
 
It finished in about 15 minutes.
Since a remote server machine was available we again ran the randomForest execution using the original 80% training data set we tried the first and it was able to finish after an hour and a half.

---

### Results
Our randomForest model using the testing data which comprised of 20% of the original training data had an OOB of 1.31%. 

Our randomForest model using the training data which comprised of 80% of the original training data had an OOB of 0.21%.  

Plots for varImpPlot for both 20% testing and 80% training were run against both models (see Figure 1 and Figure 2) which had similar orderings but not quite exactly the same results. 

If you don't see the images:

Please see figure at https://github.com/bwenzel/testDeck/blob/gh-pages/plot1.jpg

Please see figure at https://github.com/bwenzel/testDeck/blob/gh-pages/plot2.jpg
 
Using the predict function on both of the models above, both yielded the same results. The output from the table function on both randomForest predicts results, are in the next section.
 - pred=predict(modFit,real_testing);
 - table(pred,real_testing$classe)

--- Conclusions

### Conclusions
Our analysis on both randomForest models produced the same following prediction table: 

If you don't see the  image

Please see figure at https://github.com/bwenzel/testDeck/blob/gh-pages/plot3.jpg

This table was to predict the 20 different test cases, correctly.

In conclusion we were able to acurrately predict the correct Class of Excerices each Participant did by finding a Random Forest model with our desireed target of an OOB of 5% or less.     

---

### References
1.	Human Activity Recognition, http://groupware.les.inf.puc-rio.br/har. 
2.	Coursera Class "Practical Machine Learning", https://class.coursera.org/predmachlearn-002/human_grading/view/courses/972090/assessments/4/submissions
