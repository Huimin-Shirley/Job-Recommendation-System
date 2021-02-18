# Job-Recommendation-System
## Introduction
### 1.1	Objectives
Job recommendation system is an important machine learning technique in helping people especially new graduate to find satisfying and promising jobs. The objective of this project is to build a job recommendation system applying unsupervised leaning to recommend the most related and most popular jobs for job seekers according to their skills, work experience, interested positions and job descriptions. 
### 1.2	Dataset
   4 datasets used in this project are as follows:
\n The Combined_Jobs_Final.csv file: the file lists the main data for 84,090 jobs in 5470 cities. There are 23 contributes/features in the file, which include job ID, job title, description, company, city, salary, etc.
   The Job_Views.csv file: the file lists the information of jobs viewed by applicants. 
   The Experience.csv: the file containing the work experience for applicants.
   The Positions_Of_Interest.csv: contains the interested positions the applicant previously has manifested.
   So, the goal of this project is to build a job recommender using the information of 7037 applicants and 84,090 positions.
   
## Methodology
   Since we have the datasets of work experience and job preference for applicants, and the features about available positions, and these applications has more textual data and there are no ratings available for any job, a content-based collaborative filtering is an appropriate choice for this recommender system. 
   Content-based filtering uses item features to recommend other items similar to what the user likes, based on their previous actions or explicit feedback. So, we can calculate the similarity between an applicant’s previous experience and interest and available jobs, in order to recommend similar jobs to the applicant. What’s more, in order to deal with the “Cold start” problem, which means some totally new applicants has no information for reference, I plan to recommend jobs according to there popularity. I will recommend the most popular jobs to these new applicants.
   The process starts by cleaning and preparing the datasets of jobs and users respectively. Then get the numerical features from data using 2 different feature extraction techniques (TF-IDF, CountVectorizer). After that I will compute the similarity (cosine similarity) to get the similarity between previous user jobs or jobs which user has manifested interest and the available jobs. Finally, the model gets the top recommend jobs according to the score of the similarity. I will also compare the results from TF-IDF, CountVectorizer and KNN.
### 2.1	Data cleaning
   We can see from the figure bellow that several selected columns in datasets showed some missing values. So, data cleaning was implemented to deal with this problem. For the column “City”, I manually added their head quarters’ location as their location by simply searching at google, since there are only 10 companies' cities that have NaN values. For the columns “Company” and “Education.Requir” that I cannot fix, I still leave it as ‘Nan’. Because I think it will not severely influence the result.
### 2.2	Data preparation
   In order to prepare my data well for calculating similarity and clustering, preparation for data include:
   •	In order to create job corpus, combine the columns of position, company, city, emp_type, job description and title.
   •	Clean the job corpus by removing stop words.
   •	Stemming the job corpus.
   •	Transform text to lower case.
   •	Create user/applicant corpus by merging Job_View.csv, Experence.csv and Positions_Of_Interest.csv.
   •	Conduct text cleaning and transformation for user corpus as job corpus.
### 2.3	Feature extraction
   Two feature extraction techniques used for NLP are introduced as follows:
   •	CountVectorizer
   Scikit-learn’s CountVectorizer is used to convert a collection of text documents to a numerical vector of token counts by simply returning a length of the entire vocabulary and an integer count for the number of times each word appeared in the document.
   •	TF-IDF (Term Frequency - Inverse Document Frequency)
   TF-IDF is a statistical measure that evaluates how important a word is to a document in a corpus, which is a crucial algorithm to the building of item profiles in the content-based filtering recommender systems. Instead of simply returning word frequency as CountVectorizer, TF-IDF scores try to highlight words that are more meaningful, e.g. frequent in a document but not across documents.
### 2.4	Recommendation methods
(1)	Recommender by popularity
   In order to deal with “cold start” problem, we build a recommender to recommend the most popular jobs for users has none or little historical information. The process of the system is:
   •	Generate popular job ranking: Group positions of the jobs viewed by applicants and sort them to look for top k popular positions current available.
   •	Recommend top k popular jobs.
(2)	Content-based recommender
   For the users who are not cold start, we use content-based recommender. In order to calculate the similarity score and get the top recommend jobs, the cosine similarity and KNN were applied.
(3)	Hybrid recommender
   In the final recommender system, we will hybrid the popularity-based recommender and the content-based recommender. For users without sufficient information about profile, we use the popularity-based recommender. Otherwise, the content-based recommender will be used to find similarities among users and jobs information.
   
## 3.	Feature selection
### 3.1	Jobs corpus
   For creating the job corpus, we only consider 8 important columns that applicants concern: 'Job.ID', 'Title', 'Position', 'Company', 'City', 'Employment. Type', 'Education. Required', and 'Job. Description'.
### 3.2	User corpus
   For creating the user corpus, we used the following significant attributes in each file:
   •	Job views dataset: 'Applicant.ID', 'Job.ID', 'Position', 'Company' and 'City'
   •	Experience dataset: 'Applicant.ID' and 'Position.Name'
   •	Position of Interest dataset: ‘Applicant.ID’ and ‘Position.Of.Interest’
   I chose above features because these features are the main information which contains the key words for our recommendation system. Some unrelated information, such as employer name, start time, end time and update time, etc., were dropped. Meanwhile, the features having above 50% missing values were also removed, such as job requirements and salary, because they will affect the recommendation result.

## 4.	Result
   As I build recommendations systems using mainly text data and because there is no predefined testing matrix available for generating the accuracy score, I need to check the result and the performance of different recommendation systems manually.
   For test all the recommenders I randomly selected users (e.g. applicant ID = 326) from the user dataset and check the recommendation result of different models:
### 4.1	Recommendation with TF-IDF
   When we use the TF-IDF to implement feature extraction, we can see the top 10 recommended jobs are all related to java developer which is the interest of this applicant. So, the recommendation looks pretty good.
### 4.2	Recommendation with CountVectorizer
   When we use the CountVectorizer to implement feature extraction, The result is pretty similar to tfidf.
### 4.3	Recommendation using KNN
   2 of the 10 recommendation with KNN is different from previous two models. In fact, the position 9 and 10 is quite different (score close to 1 means totally different), so the system for this user only find 8 similar jobs.
### 4.4	Recommendation using popularity
   In order to test the recommendation system’s capability to deal with cold start, I also selected an applicant with no profile information (shown as below) to feed the system.We got the top 10 most popular jobs.
   
## 5.	Discussion
   In this project, I built a job recommendation system using different feature extraction techniques (TF-IDF and CountVectorizer), different machining leaning approaches (cosine similarity and KNN) and different recommendation methods (content-based and popularity-based recommender). The “cold start” problem was also considered in this system by combining two recommenders together. Among those reformers I built, the content-based recommender with TF-IDF and CountVectorizer showed a good performance in recommending the most related jobs, since it can find all similar recommendations quickly and list corresponding similarity scores. However, the content-based recommender with KNN performed not well, which could only find a small number of similar recommendations. Moreover, the established model also showed its ability in dealing with “cold start” problem using popularity-based recommender. So, I think this job recommendation system could be used in the real world and help job seekers handle their frustrating job-hunting problem.
   In my future work, I will try to find more comprehensive datasets to improve the recommendation accuracy of models, such as the resume of applicants, job postings, locations, etc. The test dataset is also needed for evaluating the performance of recommenders.





