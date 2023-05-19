# exploring-recipes-and-ratings
Thie is an exploratory data analysis project for DSC80 at UCSD.

---

## Introduction
My dataset is called recipes and ratings, which show different features regarding recipes and how users rate them. This dataset can give important insight about what users usually enjoy in a recipe and feedback about the types of recipes they like or dislike.

I am investigating this question: What tags are associated with the highest-rated recipes?

This question could show what types of recipes are popular among users, and include other insights that involve reading the tags of popular recipes. From this, we can gather trends about whether certain tags of recipes (ie romantic recipes, under 60 minutes, low-fat recipes) correlate with popularity/higher ratings of the recipes. 

The dataset ratings has 731927 rows and 5 columns- each row corresponds to an individual rating, and each row has the user_id, recipe_id, date, rating, and review. Here, the rating tells us how much the user liked the recipe, and reviews provides additional insights about how the user felt about the recipe. In the recipes dataset, there are 83782 rows and 12 columns, where each row is a recipe in the dataset. Each row has information about name, id, minutes, contributor_id, submitted, tags, nutrition, number of steps, description of steps, additional description, ingredients, and number of ingredients. Tags is most relevant to my analysis question, as I'm trying to find out how the tags on a recipe might influence or correlate to a user's review/rating. 

---

## Cleaning and EDA
I first merged the two datasets left on the recipe id, which exists as 'id' in recipes and 'recipe id' in ratings. This created a merged dataset that I used for the rest of the notebook. I then replaced all the 0 for ratings to np.nan. This is because some ratings of zero didn't necessarily indicate that the user gave a rating of 0, rather it was just unrated, and so in that case we want to count those ratings as missing values instead of actually 0.

For the purposes of this dataset, I also wanted to see the average rating per recipe, so I created a new column by grouping by the recipe id and taking the mean of the rating for that recipe id. I then mapped the average rating value across the dataframe, so that recipes of that recipe id would have the corresponding average rating value. 

### Univariate Analysis
Because I had two relevant columns that I wanted to do univariate analysis on, I did univariate analysis on both, which I have embedded here. 

<iframe src="assets/distribution_of_average_ratings.html" width=800 height=600 frameBorder=0></iframe>


The 'Distribution of Average Ratings' is a histogram containing the distribution of the ratings, and the 4.75-5.249 bin ratings are way more prevalent than any other rating. This can be interpreted as users seem to give more 4-5 star ratings on recipes when they submit ratings, meaning they generally feel positively about the recipes. 

<iframe src="assets/top_50_tags.html" width=800 height=600 frameBorder=0></iframe>


The 'Top 50 Tags' shows the top 50 most tagged tags. Here, the merged dataframe was exploded in order to have a row for every individual tag. Preparation, course, dietary, and occasion seem to be the most commonly tagged, perhaps because of the broadness of the term that may apply well to most recipes. You can see that more specific terms like chicken and fruit are less commonly tagged, as not all recipes would have chicken or fruit as an ingredient. 



### Bivariate Analysis

<iframe src="top_9_tags_average_rating.html" width=800 height=600 frameBorder=0></iframe>


Here, I conducted bivariate analysis on the variables rating and tags. I made a boxplot to show the averages and quartiles, we well as minimum/maximum of each tag's ratings. The upper quartile and median seem to all gravitate towards 5 star ratings, which is consistent with the aforementioned distribution of average ratings. We see that the minimum of each rating seems to trend towards the very low star ratings, however most of the lower quartile hovers around a neutral rating of 3. This can be interpreted as most tags being in well rated recipes, but the spread of ratings across the tags in the lower 50% clustered around neutral ratings, which less extreme ratings like 1. Users lean towards giving good or neutral ratings, with the occasionally strong negative rating. Because of file size limitations, I am unable to upload a top 50 tags plot, however they can be found in my notebook, with similar results as the top 9 plot shown here.


### Interesting Aggregates

<iframe src="assets/groupedtable.html" width=800 height=600 frameBorder=0></iframe>

This grouped table shows that average ratings for each tag. Notice that there is a unique tag for each tag item and the ratings vary from nan values to 5. I aggregated this column of ratings using the mean and not another aggregating function, because of the mean's central tendency nature. The median might've given me accurate results of the measure of central tendency as well, however a mean could best capture the skew of the ratings, if any.

Here, the top 50 rows of the grouped table is shown for reference:

| tags                |   average_ratings |
|:--------------------|------------------:|
| pot-roast           |           5       |
| snacks-sweet        |           5       |
| pork-crock-pot      |           5       |
| pickeral            |           5       |
| halloween-cakes     |           5       |
| bass                |           5       |
| beef-crock-pot      |           5       |
| somalian            |           5       |
| nepalese            |           5       |
| sudanese            |           5       |
| snacks-kid-friendly |           5       |
| crock-pot-main-dish |           5       |
| main-dish-beef      |           5       |
| namibian            |           5       |
| main-dish-chicken   |           5       |
| congolese           |           5       |
| breakfast-eggs      |           4.95833 |
| eggs-breakfast      |           4.95833 |
| halloween-cocktails |           4.95    |
| whitefish           |           4.94444 |
| angolan             |           4.92857 |
| duck-breasts        |           4.89091 |
| beef-organ-meats    |           4.88889 |
| chutneys            |           4.87543 |
| april-fools-day     |           4.86667 |
| trout               |           4.85119 |
| pork-loins-roast    |           4.83333 |
| squid               |           4.83333 |
| manicotti           |           4.82271 |
| duck                |           4.81719 |
| nigerian            |           4.8125  |
| grapes              |           4.79006 |
| czech               |           4.78935 |
| smoker              |           4.78578 |
| beijing             |           4.77778 |
| korean              |           4.76871 |
| dehydrator          |           4.76667 |
| biscotti            |           4.76618 |
| austrian            |           4.75904 |
| british-columbian   |           4.75639 |
| pakistani           |           4.75343 |
| beef-liver          |           4.75    |
| collard-greens      |           4.74838 |
| simply-potatoes2    |           4.74294 |
| rabbit              |           4.74231 |
| hungarian           |           4.74197 |
| papaya              |           4.74167 |
| icelandic           |           4.73654 |
| quiche              |           4.73611 |
| macaroni-and-cheese |           4.73435 |

---

## Assessement of Missingness
The column 'reviews' might be NMAR, because people who leave reviews may feel strongly either negatively or positively and be more inclined to take the time to write something out. On the other hand, people who have more neutral reviews may not feel super strongly and not write a review at all. It's possible to determine is this might be true by collecting additional data about how much time a user spent on that page, how many times they revisited, or other features that would show whether they had a particularly good or bad experience.

---

## Hypothesis Testing
My research question is: Is there a significant difference in average rating for recipes tagged as broccoli and recipes tagged as vegetables?

This research question was chosen because the tags vegetables and broccoli are related in that broccoli is a type of vegetable. Thus, I wondered if the specificity of the tag would influence the average rating of the recipe, or if the tags were related enough that the average ratings were similar. 

- Null hypothesis: The distribution of average ratings for recipes tagged as broccoli is the same as that of recipes tagged as vegetables.
- Alternative hypothesis: The distribution of average ratings for recipes tagged as broccoli is different from that of recipes tagged as vegetables.
- Test Statistic: I chose this test statistic because the mean is a good measure of central tendency when figuring out if these distributions are different. I chose to use the absolute difference in means because the alternative hypotehsis is A and B are different, not that A > B or vice versa. In this case, the test statistic should measure distances and should contain an absolute value.
- Significance level: I chose the p-value of 0.5 because that was the standard p-value that we used for almost every problem in class and assignments.

After conducting the hypothesis test on 1000 permutation trials and an observed statistic calculated from mean absolute difference, I obtained a p-value of 0.0092, which is lower than the alpha value or significance level of 0.05. Thus, I conclude from this test that I reject the null hypothesis. In this case, the null hypothesis fails to be supports, in which case we turn to the alternative test, which is that the distribution of average ratings for recipes tagged as broccoli is different from that of recipes tagged as vegetables. This suggests that the specificity of the tag may be a factor in deciding the average rating, however there are still many confounding and direct factors that could be explored that may impact average ratings as well.
