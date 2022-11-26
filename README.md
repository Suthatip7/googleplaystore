# googleplaystore
Objective: Make a model to predict the app rating, with other information about the app provided.

Steps to perform:
1. Load the data file using pandas. 

2. Check for null values in the data. Get the number of null values for each column.

3. Drop records with nulls in any of the columns. 

4. Variables seem to have incorrect type and inconsistent formatting. You need to fix them: 
    4.1. Size column has sizes in Kb as well as Mb. To analyze, you’ll need to convert these to numeric.
        - Extract the numeric value from the colume
        - Multiply the value by 1,000, if size is mentioned in Mb
    4.2. Reviews is a numeric field that is loaded as a string field. Convert it to numeric (int/float).
    4.3. Installs field is currently stored as string and has values like 1,000,000+. 
        - Treat 1,000,000+ as 1,000,000
        - remove ‘+’, ‘,’ from the field, convert it to integer
    4.4. Price field is a string and has $ symbol. Remove ‘$’ sign, and convert it to numeric.

5. Sanity checks:
    5.1. Average rating should be between 1 and 5 as only these values are allowed on the play store. Drop the rows that have a value outside this range.
    5.2. Reviews should not be more than installs as only those who installed can review the app. If there are any such records, drop them.
    5.3. For free apps (type = “Free”), the price should not be >0. Drop any such rows.

6. Performing univariate analysis: 
  - Boxplot for Price: Are there any outliers? Think about the price of usual apps on Play Store.
  - Boxplot for Reviews: Are there any apps with very high number of reviews? Do the values seem right?
  - Histogram for Rating: How are the ratings distributed? Is it more toward higher ratings?
  - Histogram for Size: Note down your observations for the plots made above. Which of these seem to have outliers?

 7. Outlier treatment: 
    7.1. Price: From the box plot, it seems like there are some apps with very high price. A price of $200 for an application on the Play Store is very high and suspicious!
      - Check out the records with very high price
      - Drop these as most seem to be junk apps
    7.2. Reviews: Very few apps have very high number of reviews. These are all star apps that don’t help with the analysis and, in fact, will skew it. Drop records having more than 2 million reviews.
    7.3. Installs:  There seems to be some outliers in this field too. Apps having very high number of installs should be dropped from the analysis.
      - Find out the different percentiles – 10, 25, 50, 70, 90, 95, 99
      - Decide a threshold as cutoff for outlier and drop records having values more than that

8. Bivariate analysis: Let’s look at how the available predictors relate to the variable of interest, i.e., our target variable rating. Make scatter plots (for numeric features) and box plots (for character features) to assess the relations between rating and the other features.
    - Make scatter plot/joinplot for Rating vs. Price
    - Make scatter plot/joinplot for Rating vs. Size
    - Make scatter plot/joinplot for Rating vs. Reviews
    - Make boxplot for Rating vs. Content Rating
    - Make boxplot for Ratings vs. Category

9. Data preprocessing
    8.1. Reviews and Install have some values that are still relatively very high. Before building a linear regression model, you need to reduce the skew. Apply log transformation (np.log1p) to Reviews and Installs.
    8.2. Drop columns App, Last Updated, Current Ver, and Android Ver. These variables are not useful for our task.
    8.3. Get dummy columns for Category, Genres, and Content Rating. This needs to be done as the models do not understand categorical data, and all data should be numeric. Dummy encoding is one way to convert character fields to numeric. Name of dataframe should be inp2.

10. Train test split  and apply 70-30 split. Name the new dataframes df_train and df_test.

11. Separate the dataframes into X_train, y_train, X_test, and y_test.

12 . Model building
    - Use linear regression as the technique
    - Report the R2 on the train set

13. Make predictions on test set and report R2.
