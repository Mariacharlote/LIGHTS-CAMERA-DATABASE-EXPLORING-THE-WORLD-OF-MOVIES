# LIGHTS,CAMERA,DATABASE: EXPLORING THE WORLD OF MOVIES

## Introduction: 

## Project Overview 

Microsoft would like to engage in creating original video content and maximize on the benefits that its competitors and peers are currently enjoying. 
However, they have little insider information on movie creation.

In their quest to create a new movie studio, we are tasked to identify the types of films currently at the Box office and their overal performance. 
This will include identifying other factors that may affect the performance of the different films.

This findings should result into actionable insights that can support Microsoft's management in making informed decisions on the kind of film to concentrate their financial and human resources on.

To begin this project, the first step is to gain a comprehensive understanding of both the data and the business requirements. This involves acquiring and analyzing data relevant to the project and identifying the business objectives and desired outcomes. Through this process, the researcher can establish a solid foundation and clear direction for the remainder of the project.

  


## Data Source and Data Understanding 

In order to work on this project, we acquired data from different international movie databases as highlighted below : 

1. Box Office MojoLinks to an external site.
2. Rotten TomatoesLinks to an external site.
3. TheMovieDBLinks to an external site.
4. The NumbersLinks to an external site.

The data used is contained in 3 seperate files as listed below:

1 . rt.movie_info.tsv.gz : Each record represents standard movie information. ie movie id synopsis, rating, genre,director, box office, runtime, studio.

2 . rt.reviews.tsv.gz: Each record contains reviews and ratings for the movies and the publishers of the reviews.

3 . tn.movie_budgets.csv.gz : The file contains the production budget and gross sales per movie with additional supporting data


## Business Problem Understanding 
The business problems that the project would like to answer are listed below :

1.What are the most popular genres of movies and what is the average runtime for popular movies?

2.What is the relationship between :

    a) A Movie's production budget and the revenue made
    b) Return on investment vs production

3.How do movie ratings and reviews impact : 

    a) Revenue and is there a certain threshold of ratings/reviews that is associated with higher box office success?
    b) Is there any correlation between production budget and ratings?


## Step 1: Get Intial Insights of the data

An initial examination of the raw datasets can provide valuable insights that help us in comprehending the data that we will be working with. This examination can also assist us in determining the best approach for structuring the datasets we have. 

For this project, we require to import the necessary python tools in order to access the data and view it. This include : Import pandas,csv,numoy,gzip

This stage gives us the opportunity to decide what information will be necessary for our analysis.


## Step 2: Data Cleaning and Preparation

From our previous step, we created dataframes in order to view the data in a tabular format and we identified the information inside the files.

In order to answer the business problems highlighted ealier, the information needed to answer this questions is held in different files. 

We will look into each file, clean and prepare it in readiness for our analysis to enable to derive as much insights as possible. 
Cleaning the 3 files is a lengthy task that combines the below steps for each file individually.

1 .Dropping of Unnecessary Columns and rows based on different criteria’s.
2. Filling data in empty cells.
3. Creation of new columns i.e.Return on investment(ROI).
4. Converting data into suitable data typesi.e.dates,numeric.
5. Removing of characters that don’t need to be in the columns (commas,currencysignsetc.)


## Step 3: Explore and Analyze the data

In this step, we will perform statistical anlaysis and visualize the data in order to gain insights and answer the research questions. 

The following actions will occur in this part:
* Merge the 3 dataframes
* Calculate a summary of statistics of the merged data frame
           
           *Mean
           *Median
           *Standard deviation
* Data Visualization


## Merging the cleaned dataframes
To remind ourselves, we have 3 dataframes 
* df_movie_modified
* df_budgets_modified
* df_reviews_modified

We merge the dataframes using below code and display the figures in 2 decimal places:


```python
#Merge the 3 dataframes
df_modified_merged = pd.merge(df_movie_modified, df_budgets_modified, on = 'id')
df_modified_merged = pd.merge(df_modified_merged, df_reviews_modified, on = 'id')
```


```python
# set the display format for float numbers to show 2 decimal places for easier reading and display.
#columns affected production budget, domestic/worldwide gross 
pd.options.display.float_format = '{:.2f}'.format
```


```python
#View the dataframe
df_modified_merged.head(2)
```

#### Adding columns 


```python
#Adding a new column called ROI (return on investment) to support with the analysis
df_modified_merged['ROI'] = (df_modified_merged['worldwide_gross_usd'] - df_modified_merged['production_budget_usd'])/ df_modified_merged['production_budget_usd']
```

#### Confirming the DataFrame timeframe


```python
#Lets utilise the release date. 
#1900-01-01 - The date we put as a placeholder
print("Start date: ", df_modified_merged['release_date'].min())
print("End date: ", df_modified_merged['release_date'].max())
```

    Start date:  1925-12-30 00:00:00
    End date:  2019-06-07 00:00:00
    


```python
#check for missing values
df_modified_merged.isna().sum()
```




    id                       0
    synopsis                 0
    rating_x                 0
    genre                    0
    director                 0
    writer                   0
    theater_date             0
    dvd_date                 0
    runtime                  0
    release_date             0
    movie                    0
    production_budget_usd    0
    domestic_gross_usd       0
    worldwide_gross_usd      0
    review                   0
    rating_y                 0
    fresh                    0
    critic                   0
    top_critic               0
    publisher                0
    date                     0
    ROI                      0
    dtype: int64




```python
#View a summary of the dataframe
df_modified_merged.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2356 entries, 0 to 2355
    Data columns (total 22 columns):
     #   Column                 Non-Null Count  Dtype         
    ---  ------                 --------------  -----         
     0   id                     2356 non-null   int64         
     1   synopsis               2356 non-null   object        
     2   rating_x               2356 non-null   object        
     3   genre                  2356 non-null   object        
     4   director               2356 non-null   object        
     5   writer                 2356 non-null   object        
     6   theater_date           2356 non-null   datetime64[ns]
     7   dvd_date               2356 non-null   datetime64[ns]
     8   runtime                2356 non-null   int32         
     9   release_date           2356 non-null   datetime64[ns]
     10  movie                  2356 non-null   object        
     11  production_budget_usd  2356 non-null   float64       
     12  domestic_gross_usd     2356 non-null   float64       
     13  worldwide_gross_usd    2356 non-null   float64       
     14  review                 2356 non-null   object        
     15  rating_y               2356 non-null   float64       
     16  fresh                  2356 non-null   object        
     17  critic                 2356 non-null   object        
     18  top_critic             2356 non-null   int64         
     19  publisher              2356 non-null   object        
     20  date                   2356 non-null   datetime64[ns]
     21  ROI                    2356 non-null   float64       
    dtypes: datetime64[ns](4), float64(5), int32(1), int64(2), object(10)
    memory usage: 414.1+ KB
    

#### Summary of the Merged dataframe

* The resulting merged DataFrame has 2356 rows and 22 columns. 
* The data types are 4 : datatime, float,int,object.
* We have no null figures in our data set.
* The columns have been appropriately merged and are all indicated above.
* The data occupies 414.1 KB


## Statistical Analysis and Visualization
* This is also known as univariant analysis.
* This involves generating summary statistics for the merged DataFrame.
* We will utilise df.describe() and also visualise the columns.
* The output gives a good idea of the central tendency, variability, and range of the variable we are looking into.

In df_modified merged we can only do a univariant analysis for 4 numerical columns 
- runtime 
* production_budget_usd
* domestic_gross_usd
* worldwide_gross_usd



```python
#Data Visualization : To enable us to visualize, we require to import Seaborn and Matplotlib 
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
df_modified_merged['runtime'].describe()
```




    count   2356.00
    mean     114.26
    std       16.96
    min       82.00
    25%      106.00
    50%      117.00
    75%      123.00
    max      165.00
    Name: runtime, dtype: float64



* The mean runtime is around 114 minutes, with a standard deviation of 16.96 minutes this indicates that the data is moderately spread out around the mean. 
* The minimum runtime is 82 minutes and the maximum runtime is 165 minutes.
* The median run time is 117 minites.
* The majority of the data falling between 106 and 123 minutes.


```python
# Create the histogram with a a kernel density estimate (KDE) curve
sns.histplot(data=df_modified_merged, x="runtime", color="black", kde=True)

# Add a title
plt.title("Histogram of Runtime")

# Show the plot
plt.show()
```


    
![Alt Text](https://github.com/Mariacharlote/dsc-phase-1-project-v2-4/blob/master/Histogram%20of%20runtime.png)
    



```python
df_modified_merged['production_budget_usd'].describe()
```




    count        2356.00
    mean     34623313.87
    std      42902788.42
    min          1100.00
    25%       7000000.00
    50%      20000000.00
    75%      45000000.00
    max     350000000.00
    Name: production_budget_usd, dtype: float64



* The mean production budget is about USD 34.6 million, with a standard deviation of about USD 42.9 million this indicates that the data is quite spread out around the mean. 
* The minimum production budget is about USD 1,100 and the maximum production budget is about USD 350 million.
* The median for production budget used is 20 million which falls between the 25% percentile of USD 7 million and the 75th percentile of USD 45 million.



```python
# Create the boxplot
sns.boxplot(data=df_modified_merged, x='production_budget_usd', color='blue')

# Add a title
plt.title("Boxplot of Production Budget")

# Show the plot
plt.show()
```


    
![Alt Text](https://github.com/Mariacharlote/dsc-phase-1-project-v2-4/blob/master/boxplot%20prod%20Budget.png)
    



```python
df_modified_merged['domestic_gross_usd'].describe()
```




    count        2356.00
    mean     45543251.36
    std      67253075.58
    min           527.00
    25%       4269426.00
    50%      23275439.00
    75%      58538034.50
    max     652270625.00
    Name: domestic_gross_usd, dtype: float64



* The mean domestic_gross revenue is about USD 45.5 million, with a standard deviation of about 67.3 million, this indicates that the data is quite spread out around the mean. 
* The minimum domestic_gross revenue is USD 527 and the maximum domestic_gross revenue is USD 652.3 million. 
* The majority of the domestic_gross falls between USD4.26 million and USD 58.5 million, with the median domestic_gross revenue being USD 23.3 million.


```python
# Create the boxplot
sns.boxplot(data=df_modified_merged, x='domestic_gross_usd', color='green')

# Add a title
plt.title("Boxplot of Domestic Gross Revenue")

# Show the plot
plt.show()
```


    
![Alt Text](https://github.com/Mariacharlote/dsc-phase-1-project-v2-4/blob/master/boxplot%20Domestic%20gross%20.png)
    



```python
df_modified_merged['worldwide_gross_usd'].describe()
```




    count         2356.00
    mean      98630604.24
    std      175703743.96
    min            527.00
    25%        7934697.25
    50%       38283765.00
    75%      103880027.00
    max     1648854864.00
    Name: worldwide_gross_usd, dtype: float64



* The mean worldwide gross revenue is about 98.6 million, with a standard deviation of about 175.7 million, indicating that the data is quite spread out around the mean. 
* The minimum worldwide gross value is USD 527 and the maximum worldwide gross value is USD 1.65 billion.
* The majority of the worldwide gross value falls between USD 7.9 million and USD 103.9 million, with the median value being USD 38.3 million.


```python
# Create the boxplot
sns.boxplot(data=df_modified_merged, x='worldwide_gross_usd', color='Orange')

# Add a title
plt.title("Boxplot of Worldwide Gross Revenue")

# Show the plot
plt.show()
```


    
![Alt Text](https://github.com/Mariacharlote/dsc-phase-1-project-v2-4/blob/master/boxplot%20worldwide%20gross%20.png)
    


### Lets answer our intial 3 business questions : 

    
#### What are the most popular genres of movies and what is the average runtime for popular movies?
We need to look at the movie genres and runtimes 


#### A). Most popular movie genres 


```python
genre_count = df_modified_merged['genre'].value_counts()
genre_count
```




    Drama                                                                   570
    Comedy                                                                  347
    Action and Adventure|Mystery and Suspense                               208
    Comedy|Drama                                                            206
    Art House and International|Drama|Musical and Performing Arts           156
    Comedy|Kids and Family|Romance                                          147
    Action and Adventure                                                    108
    Comedy|Mystery and Suspense|Science Fiction and Fantasy|Romance          98
    Comedy|Kids and Family                                                   53
    Action and Adventure|Classics|Drama                                      53
    Mystery and Suspense                                                     52
    Classics|Comedy|Musical and Performing Arts|Romance                      52
    Action and Adventure|Art House and International|Drama                   52
    Comedy|Romance                                                           52
    Comedy|Musical and Performing Arts                                       52
    Drama|Science Fiction and Fantasy                                        52
    Art House and International|Comedy|Drama|Musical and Performing Arts     50
    Drama|Sports and Fitness                                                 48
    Name: genre, dtype: int64




```python
#create a dataFrame of popular_genres
#sort the genre_count dataframe by count in descending order
popular_genres = genre_count.sort_values(ascending=False).head(8)
popular_genres
```




    Drama                                                              570
    Comedy                                                             347
    Action and Adventure|Mystery and Suspense                          208
    Comedy|Drama                                                       206
    Art House and International|Drama|Musical and Performing Arts      156
    Comedy|Kids and Family|Romance                                     147
    Action and Adventure                                               108
    Comedy|Mystery and Suspense|Science Fiction and Fantasy|Romance     98
    Name: genre, dtype: int64



#### B. Runtimes per genre


```python
#group the movies by genre
#calculate the mean/ average  runtime  per each genre
#Then sort values to see the figures in descending order
genre_runtimes = df_modified_merged.groupby('genre')['runtime'].mean().sort_values(ascending=False)
print(genre_runtimes)
```

    genre
    Action and Adventure|Classics|Drama                                    165.00
    Action and Adventure|Mystery and Suspense                              123.00
    Drama                                                                  122.63
    Comedy|Drama                                                           120.29
    Art House and International|Drama|Musical and Performing Arts          117.00
    Drama|Sports and Fitness                                               116.00
    Action and Adventure                                                   115.00
    Comedy|Mystery and Suspense|Science Fiction and Fantasy|Romance        113.00
    Action and Adventure|Art House and International|Drama                 110.00
    Comedy|Kids and Family|Romance                                         108.00
    Drama|Science Fiction and Fantasy                                      108.00
    Mystery and Suspense                                                   108.00
    Comedy                                                                 105.18
    Art House and International|Comedy|Drama|Musical and Performing Arts    96.00
    Comedy|Kids and Family                                                  92.00
    Comedy|Musical and Performing Arts                                      91.00
    Classics|Comedy|Musical and Performing Arts|Romance                     90.00
    Comedy|Romance                                                          86.00
    Name: runtime, dtype: float64
    

#### Most popular movie genres


```python
# create a horizontal bar plot of the top 8 genres
sns.set_style()
plt.figure(figsize=(10,6))

ax = sns.barplot(x=popular_genres.values, y=popular_genres.index, palette='Blues_r')

# set the x , y axis label and rotate the tick labels
ax.set_xlabel('Number of Movies')
plt.xticks(rotation=90)
ax.set_ylabel('Genre')
ax.set_title('Top 8 Most Popular Movie Genres')

plt.show()
```


    
![Alt Text](https://github.com/Mariacharlote/dsc-phase-1-project-v2-4/blob/master/Top8%20most%20popular%20genres.png)
    


#### Distribution of runtime


```python
# create a histogram of movie runtimes using seaborn with a KDE curve
sns.histplot(data = df_modified_merged.runtime,color = 'black', kde = True)

# set the plot title, x-axis label, y-axis label
plt.title('Distribution of Movie Runtimes')
plt.xlabel('Runtime in minutes')
plt.ylabel('Number of Movies')

# display the plot
plt.show()
```


    
![Alt Text](https://github.com/Mariacharlote/dsc-phase-1-project-v2-4/blob/master/distribution%20on%20movieruntimes.png)
    


##### Top 8 Genres by Average Runtime


```python
# calculate the average runtime for each genre, sort by descending order, and select the top 8
genre_runtimes = df_modified_merged.groupby('genre')['runtime'].mean().sort_values(ascending=False).head(8)

# create a horizontal bar plot using seaborn
sns.barplot(x=genre_runtimes, y=genre_runtimes.index, palette='GnBu')

# set the plot title, x-axis label, and y-axis label
plt.title('Top 8 Genres by Average Runtime')
plt.xlabel('Average Runtime (minutes)')
plt.ylabel('Genre')

# set the figure size
plt.figure(figsize=(10,6))

# display the plot
plt.show()

```


    
![Alt Text](https://github.com/Mariacharlote/dsc-phase-1-project-v2-4/blob/master/top%208%20average%20runtimes.png)
    



    <Figure size 720x432 with 0 Axes>


##### What is the relationship between a movie's production budget and the revenue made?

Its an expectation that movies with higher production budgets will generate higher revenues, but this relationship is not always straightforward. A movie's success is influenced by various factors, such as the quality of the script, the skill of the director, the popularity of the actors, and the timing of the release, among others. 

To identify the relationship between the production budget and revenue in our data set we can create a scatter plot of budget vs revenues. 

This would allows us to inspect trends and patterns.
We can also quantify the return on investment (ROI) for each movie. This can be calculated as the ratio of worldwide gross revenue to production budget.

In the process this would allow us to compare the profitability of movies with different budgets and revenue levels.



##### Production budget vs. worldwide gross revenue


```python
#Create a scatter plot of production budget vs. worldwide gross revenue
# create a scatter plot using seaborn
sns.scatterplot(x='production_budget_usd', y='worldwide_gross_usd', data=df_modified_merged, color='green')

# set the plot title, x-axis label, and y-axis label
plt.title('Production Budget vs. Worldwide Gross Revenue')
plt.xlabel('Budget (USD)')
plt.ylabel('Gross Revenue (USD)')

# display the plot
plt.show()

```


    
![Alt Text](https://github.com/Mariacharlote/dsc-phase-1-project-v2-4/blob/master/budget%20vs%20worldwide.png)
    


It can be noted, that not always that movies with higher production budgets will generate high revenues.  
For Example : Movies with a budget of USD 1.5 Million dollars had varying revenues. Meaning other factors were in play.


#### Movies based on ROI 


```python
#seems we have an outlier in the data 
# Remove outlier from dataframe by create a new dataframe 
#that contains all rows from df_modified_merged where the ROI value is less than 150.
df_modified_merged = df_modified_merged[df_modified_merged['ROI'] <=150]
```


```python
#Identify the top 1000 movies based of their Return on investment
df_top_1000 = df_modified_merged.sort_values(by='ROI', ascending=False).head(1000)

```


```python
# Create a seaborn scatter plot of ROI vs. production budget 
sns.scatterplot(x='production_budget_usd', y='ROI', data=df_top_1000, color='blue')

# set the plot title, x-axis label, and y-axis label
plt.title('ROI vs Production Budget')
plt.xlabel('Production Budget (USD)')
plt.ylabel('ROI')

# display the plot
plt.show()
```


    
![Alt Text](https://github.com/Mariacharlote/dsc-phase-1-project-v2-4/blob/master/ROI%20vs%20Production.png)
    


It can be noted from above, the Return on investment (ROI) is not guaranteed by the production budget used . 

The scatter plot shows very little correlation between the ROI and investment put in. 


##### How do movie ratings and reviews impact the overal revenue?



```python
corr_coef = np.corrcoef(df_modified_merged['worldwide_gross_usd'], df_modified_merged['rating_y'])[0, 1]
corr_coef
```




    -0.01802876105675924



* The value of -0.018358351301180985 indicates a very weak negative correlation between the two variables. 
* This means that there is a slight tendency for movies with higher ratings to have slightly lower worldwide gross revenue, but the relationship is not very strong.

* A weak and negative correlation was observed between this 2 variables 

Movies will little to no revenue gathered as many ratings as movies with higher revenues.

Movies with higher revenues gathered less ratings than would have been expected. 



```python
# create a scatter plot of Revenue vs. Ratings
sns.scatterplot(x='worldwide_gross_usd', y= 'rating_y', data=df_modified_merged,color = 'black')

# set the x-axis label and rotate the tick labels
plt.xlabel('Worldwide Gross Revenue (USD)')

# set the y-axis label
plt.ylabel('Rating')

# set the title of the plot
plt.title('Worldwide Gross Revenue Vs. Rating')

# display the plot
plt.show()
```


    
![Alt Text](https://github.com/Mariacharlote/dsc-phase-1-project-v2-4/blob/master/Worldwide%20gross%20vs%20rating.png)
    


##### is there a correlation between production budgets and ratings? 


```python
corr_coef = np.corrcoef(df_modified_merged['production_budget_usd'], df_modified_merged['rating_y'])[0, 1]
corr_coef
```




    0.007785645101974284



* The value of 0.0071651084685253495 indicates a very weak positive correlation between the two variables. 
* This means that there is a slight tendency for movies with higher production budgets to have slightly higher ratings, but the relationship is not very strong.
* lets plot and see.


```python
# create a scatter plot of Revenue vs. Ratings
sns.scatterplot(x='production_budget_usd', y= 'rating_y', data=df_modified_merged)

# set the x-axis label and rotate the tick labels
plt.xlabel('Production_Budget(USD)')

# set the y-axis label
plt.ylabel('Ratings')

# set the title of the plot
plt.title('Production Budget Vs. Ratings')

# display the plot
plt.show()
```


![Alt Text](https://github.com/Mariacharlote/dsc-phase-1-project-v2-4/blob/master/production%20budget%20vs%20ratings.png)
    


## Step 4: Summary of Results

After analyzing the DataFrame df_modified_merged, several interesting results were found.
 
 * The average runtime for movies in the dataset was 114  minutes.
 
 * No movie had a runtime of less than 82 min
 
 * The Average movie production budget spent by movie makers in this dataset was 34.6 Million however, majority of movies spent between USD 7 million and USD 45   million.
 
 * The highest budget amount was USD 350 Million.
 
 * A movie's worldwide revenue can range from USD 7 Million to USD 103 Million for majority of the movie makers however, one movie maker made as low as USD 527
 
 * The highest worldwide gross revenue registered was USD 1.65Billion.
 
 * Drama genre was the most popular genre with over 500 number of movies followed by Comedy with 350.
 
 * Action & Adventure|Classics| Drama had the highest average runtime among all the genres present. 
 
* Drama came third followed by Comedy|Drama which were very near the mean of 114 minutes.

* On carrying out the bivariant analysis:
   * It was concluded that higher production budgets will not always guarantee  high revenues or return on investment.
   
   * There is a negative weak correlation observed between worldwide gross revenue and ratings.
   
   * A slight tendency was noted on movies with higher production budgets to have slightly higher ratings, but the relationship is not very strong.


 

## Step 4 b) Recommendations to consider:  

##### Genre  and Runtime
* To Invest in drama and comedy movies as they are the most popular genres.
* To Keep in mind the appropriate length of each movie . The average mean is 114 minutes

##### Budgets & Ratings
* Although the average production budget spent by movie makers in this dataset was 34.6 Million, consider that the majority of movies spent between USD 7 million and USD 45 million. 
* This means a lower budget can always be utilized. 
* Microsoft should not solely rely on higher production budgets as a guarantee for higher ratings or revenues.

##### Revenue & Ratings
* Microsoft should keep in mind that there is a weak negative correlation between worldwide gross revenue and movie ratings.
* This means that having high ratings does not necessarily guarantee high revenues.
* Thus should not rely heavily on ratings.


## Conclusion

Finally, Microsoft should keep in mind that while the majority of movies in the dataset generated between USD 7 Million and USD 103 Million in worldwide revenue, the highest worldwide gross revenue registered was USD 1.65 Billion. 
Therefore, it is important to remain open to the possibility of high revenue generation, and not limit investment opportunities based on past revenue trends.



```python

```
