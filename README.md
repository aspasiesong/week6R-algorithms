# ANA505 Week 6 R: Algorithms
a repository to hold my week 6 R activity code - Topic: algorithms

SVM Algorithm: <br>
--------------
SVM classifies data using a hyperplane, or simply a line if there are only 2 categories. <br>
The SVM script takes a look at the iris dataset and creates an SVM model. From looking at the scatterplots and density plots, we can see that the Setosa species can be identified with high confidence, whereas there is some overlap between the Versicolor and Virginica species. With just the petal length and petal width, we can quite accurately classify the three species. <br>
<br>
**SVM Accuracy = 0.9733 = 97.33%**
```
sum(diag(tab)/sum(tab))
```

K Means Algorithm: <br>
--------------
K means creates clusters out of a dataset to group members by similarities. <br>
The K means script normalizes the attributes to obtain values between 0 and 1. From there, the K means algorithm is applied and 3 centroids are defined. <br>
<br>
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
<br>
**C50 Accuracy = 0.9556 = 95.56%** 
```
mean(testData$Species == predict(dtModel, testData))
```
<br>

**CODE SCRIPTS**

SVM Algorithm: 
--------------
```
#e1071 will be used for Support Vector Classification.
install.packages("e1071")
install.packages("GGally")
install.packages("ggplot2")

library(e1071)
library(GGally)
library(ggplot2)

#get the dataset
data(iris)

#explore the data
str(iris)
head(iris,3)

#Create the SVM model
svm_model <- svm(Species ~ ., data=iris,
                 kernel="radial") #linear/polynomial/sigmoid

#Lets have a closer look at the parameters 
#and judge before hand if a good model can be created or not.
ggpairs(iris, ggplot2::aes(colour = Species, alpha = 0.4))

#We can clearly see from the Histograms of Petal.length 
#and Petal.width that we can clearly seperate out Setosa species with very high confidence.

#However, Versicolor and Virginica Species are overlapped. 
#If we look at the scatterplot of Sepal.Length vs Petal.Length 
#and Petal.Width vs Petal.Length, 
#we can distintly see a seperator that can be draw between the groups of Species.

#Looks like we can just use Petal.Width and Petal.Length as parameters 
#and come with a good model. SVM seems to be a very good model for this type of data.

plot(svm_model, data=iris,
     Petal.Width~Petal.Length,
     slice = list(Sepal.Width=3, Sepal.Length=4) 
)

#from the graph you can see data, support vector(represented by cross sign) 
#and decision boundry, belong to 3 types of species

#White color represented predicted class for second species(versicolor)

#Pink color represented predicted class for third species(virginica)

#Also we have 52 Support vector, 
#8 of them belongs to first species
#(You can see 8 cross in first class), 
#22 of them belongs to second species, 
#21 of them belongs to third species.

#Predict each Species
#Confusion matrix and missclassification error
pred = predict(svm_model,iris)
tab = table(Predicted=pred, Actual = iris$Species)
tab

#Get missclassification rate
1-sum(diag(tab)/sum(tab))

#How did the model do?
#What is the accuracy rate?
#ANSWER: 97.33%
```

K Means Algorithm: 
--------------
```
#K-Means Clustering

require("datasets")
data("iris") # load Iris Dataset
str(iris) #view structure of dataset

summary(iris) #view statistical summary of dataset

head(iris, 3) #view top  rows of dataset

#Preprocess the dataset
#Since clustering is a type of Unsupervised Learning, 
#we would not require Class Label(output) during execution of our algorithm. 
#We will, therefore, remove Class Attribute “Species” and store it in another variable. 
#We would then normalize the attributes between 0 and 1 using our own function.

iris.new<- iris[,c(1,2,3,4)]
iris.class<- iris[,"Species"]
head(iris.new, 3)

head(iris.class, 3)

normalize <- function(x){
  return ((x-min(x))/(max(x)-min(x)))
}

iris.new$Sepal.Length<- normalize(iris.new$Sepal.Length)
iris.new$Sepal.Width<- normalize(iris.new$Sepal.Width)
iris.new$Petal.Length<- normalize(iris.new$Petal.Length)
iris.new$Petal.Width<- normalize(iris.new$Petal.Width)
head(iris.new)

#Apply the K-means clustering algorithm
result<- kmeans(iris.new,3) #aplly k-means algorithm with no. of centroids(k)=3
result$size # gives no. of records in each cluster

result$centers # gives value of cluster center datapoint value(3 centers for k=3)

result$cluster #gives cluster vector showing the custer where each record falls

#Verify results of clustering
par(mfrow=c(2,2), mar=c(5,4,2,2))
plot(iris.new[c(1,2)], col=result$cluster)# Plot to see how Sepal.Length and Sepal.Width data points have been distributed in clusters
plot(iris.new[c(1,2)], col=iris.class)# Plot to see how Sepal.Length and Sepal.Width data points have been distributed originally as per "class" attribute in dataset
plot(iris.new[c(3,4)], col=result$cluster)# Plot to see how Petal.Length and Petal.Width data points have been distributed in clusters
plot(iris.new[c(3,4)], col=iris.class)

table(result$cluster,iris.class)

#Results of the table show that Cluster 1 corresponds to Virginica, 
#Cluster 2 corresponds to Versicolor 
#and Cluster 3 to Setosa.

#Total number of correctly classified instances are: 36 + 47 + 50= 133
#Total number of incorrectly classified instances are: 3 + 14= 17

#How did the model do?
#TASK: Accuracy = number of correctly classified/(total classified) = ?
#i.e our model has achieved ?% accuracy!
pred_acc <- data.frame(table(result$cluster,iris.class))
accuracy <- sum(pred_acc[2,3], pred_acc[4,3], pred_acc[9,3])/150
print(accuracy)
#ANSWER: 88.67% 
```

C50 Algorithm: 
--------------
```
#The C5.0 algorithm
#First install these packages
#install.packages("C50") - this didn't work for me
install.packages("C50", repos="http://R-Forge.R-project.org")
install.packages("dplyr")
library(dplyr)

#get the iris dataset (for more info https://en.wikipedia.org/wiki/Iris_flower_data_set)
iris

head(iris,4)
dim(iris)

#we need to divide our data into training data and test data. 
#C5.0 is a classifier, so you’ll be teaching it how to classify the different species of irises using the training data.
library(C50)

#Splitting data set into traing and testing.
#Splitting data based on the species
iris_setosa <- iris[iris$Species == "setosa", ]
iris_versicolor <- iris[iris$Species == "versicolor",]
iris_virginica <- iris[iris$Species == "virginica",]

#splitting data sequentially *optional
iris_train <- rbind(iris_setosa[1:35,], iris_versicolor[1:35,], iris_virginica[1:35,])
iris_test <- rbind(iris_setosa[36:50,], iris_versicolor[36:50,], iris_virginica[36:50,])

#spliting randomly
#install caret lib which is used to split the dataset
install.packages("caret")
library(caret)

install.packages("lattice")
install.packages("ggplot2")

library(lattice)
library(ggplot2)

attach(iris)
inTrainingData <- createDataPartition(y= Species, p=0.70, list = FALSE)
trainData <- iris[inTrainingData,]
testData <- iris[-inTrainingData,]

#Build the model on the training data
library(C50)
dtModel <- C5.0(trainData[,-5], trainData$Species)
plot(dtModel)

#Checking accuracy of the training data model
#The predict() function takes your model, the test data 
#and one parameter that tells it to guess the class 
#(in this case, the model indicates species).
predict(dtModel, testData)

summary(testData)

(testData$Species == predict(dtModel, testData))

mean(testData$Species == predict(dtModel, testData))

#cross table
install.packages("gmodels")
library(gmodels)
CrossTable(testData$Species,predict(dtModel, testData))

CrossTable(testData$Species == predict(dtModel, testData))

#How did the model do? 
#TASK: What percent of cases were correctly classified?
#ANSWER: 95.56%
```


