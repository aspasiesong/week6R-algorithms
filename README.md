# ANA505 Week 6 R: Algorithms
a repository to hold my week 6 R activity code - Topic: algorithms

SVM Algorithm: <br>
--------------
SVM classifies data using a hyperplane, or simply a line if there are only 2 categories. <br>
The SVM script takes a look at the iris dataset and creates an SVM model. From looking at the scatterplots and density plots, we can see that the Setosa species can be identified with high confidence, whereas there is some overlap between the Versicolor and Virginica species. With just the petal length and petal width, we can quite accurately classify the three species. <br>
**SVM Accuracy = 0.9733 = 97.33%**
```
sum(diag(tab)/sum(tab))
```

K Means Algorithm: <br>
--------------
K means creates clusters out of a dataset to group members by similarities. <br>
The K means script normalizes the attributes to obtain values between 0 and 1. From there, the K means algorithm is applied and 3 centroids are defined. 
**K Means Accuracy = 0.8867 = 88.67%** 
```
pred_acc <- data.frame(table(result$cluster,iris.class))
accuracy <- sum(pred_acc[2,3], pred_acc[4,3], pred_acc[9,3])/150
print(accuracy)
```

C50 Algorithm: <br>
--------------
C50 is an extension of the C4.5 algorithm that classifies data using decision trees. The algorithm houses a series of decision trees that form a flowchart, where at each point in the flowchart the data is classified into one or another category based on for example an attribute. <br>
The script uses training data to teach the model how to classify the different species of irises. <br>
**C50 Accuracy = 0.9556 = 95.56%** 
```
mean(testData$Species == predict(dtModel, testData))
```
