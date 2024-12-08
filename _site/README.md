# Recipes Rating Analysis

**Name(s)**: Grace Gao & Faith Jones

**Website Link**: https://gracegyq.github.io/recipes_rating_analysis/

## Introduction

This data analysis centers around recipies and ratings found on food.com posted since 2008. There were two relevant datasets for this problem which was the recipes dataset with 83782 rows and 12 columns and the interactions dataset with 73197 rows and 5 columns which represents the rating and reviews for recipes. Together, the datasets include many columns about the recipe like name, ingredients, cooking time, ratings and more. 

To examine this data set we came up with an overarching question: **How does different recipe types and factors affect the rating?**

To investigate this question, we first cleaned the data and conducted exploratory data analysis to try and identify potential trends or important factors to our question. Next, we built a baseline model to start trying to predict recipe's ratings. After this, we had many iterations and variations of the model as we attempted to improve our prediction accuracy until we finally landed on a final model for predicting recipe rating.

The original data sets contained 16 columns, however as part of our cleaning we both created new relevant columns and selected the pertinent columns for the sake of our analysis. The table below is a list of the pertinent columns along with a description of each:

<table>
  <thead>
    <tr>
      <th>Column</th>
      <th>Decsription</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>


## Data Cleaning and Exploratory Data Analysis

Before we started any analysis we first needed to clean the data.

### Data Cleaning

To begin cleaning the data, we first performed a left merge on the recipe ID for the recipes and interactions dataset with the recipes being on the left. Merging the data allows us to then be able to use the factors in the recipe dataframe to prdict the ratings from the interactions dataframe.

Once merged, we dealed with any missingness in our data. First we checked for missingness in the data. We found that the only columns in the dataframe that had missing values were only the 'name' and 'description' columns in the dataset. For the name column, we decided to leave it as NaN because we are inferring that it is a recipe with all special characters for the name, so, even though the recipe does not have a name it can still be helpful in our attempts to predict ratings based on the other columns. Further, we decided to do nothing for the description because as stated above, we didn't include the description column in our pertinent dataframes. Additionally, when analyzing the data we noticed some of the raings had a 0, which is not possible on food.com. For all the 0 values, we decided to fill these values with NaN because the cause of these 0 star ratings is that someone left a review/comment on the recipe, and did not provide a rating. Therefore, having these recipes with a rating of NaN makes more sense since they did not have a rating as opposed to getting a rating of 0.

Next, we added a new column to find the average rating per recipe since as the dataset was the same recipe could have multiple ratings. We did this by grouping the recipe ids and finding the mean of the ratings. Then we converted this series to a dataframe and merged it with the current dataframe we were working with. This affected our analysis because it allowed us to be able to predict the recipes overall rating instead of focusing on individual reviews and recipes since we want a more holistic prediction for the recipes. To further build on the ratings, we then added a column for the rating_category which rounds the average rating to an integer value in order to use these distinct categories as classes for our models. We decided to drop the rating_categories that were NaN because without knowing which category it’s in, we can’t test the accuracy of the prediction for that column.

The last part of our cleaning concerned cleaning the ‘nutrition’ column originally from the recipes dataset. We first converted the data for this column into a list. After then we extracted all of the information from nutrition out into its relevant columns which are ‘calories’, ‘total fat (PDV)’, ‘sugar (PDV)’, ‘sodium (PDV)’, ‘protein (PDV)’, ‘saturated fat (PDV)’, and ‘carbohydrates (PDV)’. This is helpful because it allows us to use these factors, like calories, to build our model and help predict the ratings.

ADD HEAD OF CLEANED DF 

### Exploratory Data Analysis

#### Distribution of Recipe Ratings

To start understanding our data set, we made many different graphs to visualize trends in our data. One of the most prevalent distributions to our analysis is the distribution of the recipe ratings which you can find below:

<iframe
  src="assets/ratings_distribution.html" ##change this please
  width="800"
  height="600"
  frameborder="0"
></iframe>

From the plot we can see that most of the ratings are between 4 and 5, also please note that the bin from 5 to 6 are all 5 ratings because there can't be more than a 5 star rating. For our analysis this might mean that the dataset is baised towards the higher ratings, like 4 or 5, because a significant portion of the dataset is 4 to 5 ratings.

## Framing a Prediction Problem
**Prediction Problem:**
We are attempting to use the columns with information regarding calories, number of steps it takes, number of ingredients, and cooking time to predict the rating of the recipe, and weo performed multiclass classification for our prediction models. 

**Evaluation Metric:** 
We used a combination of overall accuracy as well as the precision level per class. The overall accuuracy evaluated how well the model did in terms of predicting the correct class given information from the four columns described above. Additionally, we looked at the confusiono matrix, particularlyy the precision level for each class, as an additional metric because that would provide insight on whether the accuracy is from actually predicting all the classes correctly or just from predicting all the data points to be of the class with the highest frequency, which would still result in a decent overall accuracy if that class has significantly more data points than the rest. 


## Baseline Model
For our baseline model, we chose to use a `RandomForestClassifier` as our training model, which uses a collection of decision trees to make the classification prediction. The features we used to train this model are `calories`, which represents the number of calories for each recipe, and `minutes`, which is the number of steps it takes to finish the recipe, boht of which we thought have the potential to make reasonable predictions for the rating of a recipe. Both of the features are quantitative data columns. We considered using the categorical columns, such as ingredients and tags; however, after thorough exploration, we decided that performing one-hot-encoding or other transformation functions on the categorical variables would not be very effective given the fact that there are too many options for ingredients or tags. We did not perform `StandardScaler()` on the two features chosen because `RandomForestClassifier` is not very sensitive to the scale of data since it is a threshold-based approach. 

We believe that our model is decent, but it revealed a signficant problem in out datasets that makes classification much more challenging, which is the problem of class imbalance. Our model's accuracy is approximately 0.5736 on used on the unseen testing set, which is not a bad prediction accuracy; however, when looking at the precision parameter for each of the rating classes in the classification report, the precision of ratings of 1, 2, and 3 are all 0.0 or near 0 while the precision for ratings of 4 and especially 5 are signficantly higher. Looking more specifically at the distribution of predictions, it seems that the model mostly predicts that all the data points have a 5-rating. The decently high accuracy of the model is simply from the fact that the entire dataset, and by extension the test set as well, have a significantly higher proportion of 5-ratings than all other ratings. This suggests that our current model is not the best predictor model because it is taking advantage of the class imbalance and not faily predicting results for all five of the distinct classes. 

## Final Model



oversampling underrepresented classes