This project includes all the code used to analyze the data of the [Yelp Dataset Challenge](http://www.yelp.com/dataset_challenge) during the Data Mining Capstone Project of the Data Mining Specialization in Coursera.

The project is organized in folders as folows.

- data. This folder should contain the Yelp dataset. But due to the data license agreement, it cannot be distributed.
- scripts. R scripts used to analyze the data.
- results. This folder will hold the intermediate results generated by the scripts in the script folder.
- reports. Project milestone reports.
- tools. Downloaded command line tools.

# Instructions

To reproduce the analysis, first unzip the Yelp JSON files in the data folder and then run the following scripts for each task.

## Task 1. Exploratory analysis.

1. **scripts/exploratory/read.data.R** Reads Yelp data in JSON format and saves the resulting data.frames in RData files.

2. **scripts/exploratory/restaurant_id.R** Filters business by category to keep only those labeled as Restaurants and then filters the reviews to keep reviews for restaurants. Stored the ids of the filtered businesses and reviews.

3. **scripts/exploratory/review.model.language.R** Computes a unigram language model for a corpus of Yelp reviews.

4. **scripts/exploratory/restaurants_model_language.R** Subsets the unigram model language to keep only a model language for a corpus of restaurant reviews. The vocabulary is also reduced to the most frequent words that account for 99% of the corpus.

5. **scripts/exploratory/topicmodel.R** Computes a topic model for the corpus of restaurant reviews.

6. **scripts/exploratory/data_by_rating.R** Splits the corpus of restaurant reviews into two corpora. One for positive reviews (stars >3) and another for negative reviews (stars < 3).

7. **scripts/exploratory/topicmodel_positive_negative.R** Computes a topic model for the positive and for the negative restaurant reviews corpora.

8. **scripts/exploratory/subset_useful_restaurant_reviews.R** Selects restaurant reviews that are useful. One review is useful if has been upvoted as usefull at least once.

9. **reports/01-Exploratory.Rmd** knits the exploratory analysis report 

## Task 2. Cuisine Clustering and Map Construction

1.  **scripts/cuisine_clustering/data_by_cuisine.R** Splits the reviews in restaurant by restaurant category. It removes those categories that are not clearly related to restaurants like Taxi and Automotive.

2.  **scripts/cuisine_clustering/cuisines_subsample.R** Loads the document-term matrix of restaurant reviews and saves a subset dtm of positive useful reviews for a subset of restaurant categories.

3. **scripts/cuisine_clustering/dtm_by_cuisine_aggregated.R** Aggregates the DTM of positive useful restaurant reviews by cuisine label. For that it concatenates all the documents for each label.

4. **scripts/cuisine_clustering/cuisine_aggregated_topic_model.R** Computes topic model with k=20 for dtm of concatenated positive useful restaurant reviews of a subset of restaurant categories.

5. **scripts/cuisine_clustering/similarities_aggregated.R** Computes similarities between cuisine categories using concatenated positive useful restaurant reviews. It tries three approaches: 1. cosine similarity of raw Term Frequencies. 2. BM25+ 3. Cosine similarities of probability distribution of each cuisine over the topics of a topic model fitted to the corpus using LDA.

6. **scripts/cuisine_clustering/topicmodel_by_cuisine.R** # Computes topic model with k=100 for dtm of positive useful restaurant reviews of a subset of restaurant categories.

7. **scripts/cuisine_clustering/similarities_not_aggregated.R** Computes similarities between cuisine categories using positive useful restaurant reviews. It tries three approaches: 1) Cosine similarities of probability distribution of each cuisine over the topics of a topic model fitted to the corpus using LDA. 2) cosine similarity of raw Term Frequencies. 3) BM25+ This script is computationally intensive and requires to be run on a computer with GPU computing capabilities.

8. **scripts/cuisine_clustering/cuisine_networks.R** Extracts backbone graphs from similarity matrices to create cuisine maps and applies community detection algorithms to cluster the cuisines.

9. **reports/02-Cuisine_Map.Rmd** knits a report about cuisine maps.

## Task 3. Dish Discovery

1. Enter on each of the cuisine folders in **scripts/dish_recognition/** and run the corresponding **cuisine_subset.R** script to create a corpus for each cuisine.

2. Copy from each of the cuisines folders in **results/dish_discovery/** the **cuisine_positive_useful_corpus.txt** and paste in the data corresponding input folder of TopMine and SegPhrase.

3. Run TopMine and SeghPhrase on each of the corpora. Only change the input files and keep all the remaining parameters as they are. The labeled samples for SegPhrase are located in **results/manualAnnotationTask/**

4. Enter on each of the cuisine folders in **scripts/dish_recognition/** and run the corresponding **cuisine_word2vect.R** script to run the Word2Vect algorithm on each of the corpora.

5. Run **scripts/dish_recognition/cross_cuisines.R** to combine all the algorithms results for each cuisine.

6. Run **reports/03-Dish_Discovery.Rmd** to knit a report about the results.

## Task 4. Dish ranking

1. Run **scripts/dish_ranking/dishes_count.R** to ompute the document frequency of each dish in the restaurant reviews corpus.

2. Run **scripts/dish_ranking/topicmodel.R** to compute a topic model for the corpus of restaurant reviews. 3 topics
3. Run **scripts/dish_ranking/dish_rankings.R** to compute dish rankings for 6 cuisines using 4 scores: document frequency, average review stars, and versions of the two scores weighted by the probability that the reviews belong to a food topic.

## Task 5. Restaurant recommender

1. Run **scripts/restaurant_recommendation/restaurant_dishes_ratings.R** to compute the dishes-restaurants ratings matrix for 6 cuisines and for the 4 scored used to rank dishes.

2. Run **reports/04-05-Dish_Ranking_Restaurant_Recommendation.Rmd** to knit a report about the results of tasks 4 and 5.

## Task 6. Higiene prediction

1. Run **scripts/hygiene_prediction/read_data.R** to read the data and store it in RData files.
2. Run **scripts/hygiene_prediction/language_model.R** to build an unigram language model for the reviews.
3. Run **scripts/hygiene_prediction/reduced_language_model.R** to create a reduced version of the unigram language model by keeping only those terms that account for 99% of the words in the corpus.
4. Run **scripts/hygiene_prediction/topicmodel.R** to compute a topic model from the non-reduced laguage model
5. Run **scripts/hygiene_prediction/training_test_dtm_unigram.R** to concatenate the reduced unigram language model to other features in the training and test set.
6. Run **scripts/hygiene_prediction/training_test_topic_model_50.R** to concatenate the results of a topic model to other features in the training and test set.
7. Run **scripts/hygiene_prediction/training_test_topic_model_label1_100.R** to concatenate the results of a topic model trained only on the positive training samples to other features in the traning and test set.
8. Run **scripts/hygiene_prediction/random_forest.R** to train a classifier using random forest
9. Run **scripts/hygiene_prediction/svm.R** to trains a classifier using SVM
10. Run **scripts/hygiene_prediction/logistic.R** to trains a classifier using logistic regression.
11. Run **reports/06-Hygiene_prediction.Rmd** to generate a report for this task.

## Final Report

Run **reports/07-Final_Report.Rmd** to general the final report.
