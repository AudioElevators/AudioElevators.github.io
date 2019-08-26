---
layout: post
title:      "A Look Into Predictive Models"
date:       2019-08-25 21:42:42 -0400
permalink:  a_look_into_predictive_models
---

While I wrap up my first project of this class, I took some time today to reflect on all that I've learned as I near the deadline. Not only have I learned and applied impertient technical and nontechnical aspects of data science, but I've gained greater and yet, at the same time, less confidence in my coding abilities. In this blog post, I'll go over some of my highlights (and lowlights?) from this project.

Let's start at the beginning.

Around 2pm two weeks ago, Abinheet, one of my instructors, gave my peers and I a briefing of the Module 1 Project. As he listed the requirements and his expectations for us, I sat in doubt. A multivari-- what?! I was still working on pronouncing multicollinearity from section 8 and now I needed to be able to apply the entire module in one project. I tried not to freak out. Deep breaths. Don't panic. Baby steps. With a redbull by my side, I delved into what would be one of the most mentally challenging things I've ever done. 

Before I began the coding process, I watched a handful of videos on the project. There, I learned that cleaning the data would take around 60% of the time spent on the project -- 60%! I was surprised that most of my time would be ensuring the data was formatted correctly for my model. So, I went right to section 5 and started cleaning.

And cleaning.

   And cleaning.

        And... cleaning.

**Lesson 1**: Data, often, does not get along with models. 

Here's some of the stuff I did to mend this rocky relationship:

*Dealing with ?'s by replacing them with the median of the column.*

```
df['sqft_basement'].replace('?', np.nan , inplace=True)
df['sqft_basement'].fillna(df['sqft_basement'].median(), inplace=True)
```

The square footage of the basement was the trickiest column I had to deal with because I had to replace ?'s with NaNs and *then* fill these NaNs with the median, allowing me to keep the column and its data for EDA. I ended up not using this guy in my model due to its high p-value, but I wouldn't have known its insignificance if I hadn't cleaned it up and explored it first!

*Transforming categorical variables by creating dummy variables...*

`grade_dummies = pd.get_dummies(df["grade"], prefix="grd", drop_first=True)`

The grade column was a categorical variable since each house fit in a grade (category) from a rating scale of 1 to 13, so my model wouldn't be able to run it unless I seperated the grade column into individual rating columns. 

*...and by using one-hot encoding.*

```
waterfront = ["Yes", "No", "Yes", "No", "Yes", "Yes", "No", "No",]
waterfront_series = pd.Series(waterfront)
cat_waterfront = waterfront_series.astype('category')
cat_waterfront
pd.get_dummies(cat_waterfront)
```

With waterfront being a categorical variable with only two categories, waterfront or no waterfront, I used one-hot encoding to make them 1s and 0s, respectively. 

60% of my time, making sure the data is usable.

Cool.

#justdatascientistthings?

Okay, we'll leave that hashtag in 2016.

Next comes the fun part. Exploration! I was so excited to apply Matplotlib and Seaborn in visualizing the trends and relationships within and between these 19 variables in the King County Housing dataset. As a visual learner, being able to create visuals to communicate the information in the data was extremely useful in understanding how to build my model to accurately predict the price of a house. Not only that, Seborn makes them look really cool.

**Lesson 2**: I love graphs. 

Here are some of my favorite visualizations:

*A scatterplot of sale price versus the square footage of houses in the dataset.*

![](https://i.imgur.com/2u3dxaO.png)

Scatterplots are neat for showing the relationship between two variables i.e. if they're related and if they're correlated. Here, we can see that the sale price and the square footage of a house are related and there is a pretty strong upward trend. 

*A heat map of the latitude and longitude data columns.*

![](https://i.imgur.com/WTSQwLO.png)

Wow! Here we can visually see where houses get pricier with location, especially near the water. How do we know that it's the water? The coordinates of houses have made dot art of the King County area. Here's a picture of King County for comparison.

![](https://www.kingcounty.gov/about/region/~/media/about/maps/KC_simplemap_Oct2013.ashx) 

*Heat map of correlation between variables.*

![](https://i.imgur.com/xac49OE.png)

Lastly, we come to most technical part of this post: making the model. I have to say, this was tricky. Name errors, syntax errors, errors that took up most of my computer screen -- you name it, I probably came across it. Each error felt like a hit to my health bar; sour skittles and energy drinks could only do so much to restore my morale. But, I trudged on. Reached out to instructors and peers, cried a little, worked into crazy hours of the night, thanked God for StackOverflow, and constantly reminded myself that yes, I *can* do this. So I did. 

My first model was far from perfect: large standard errors, multiple p-values of exactly 0 and greater than 0.05, and a low R-squared value are just a few of the problems I encountered. So, like I had done many, many times, I went back to module 1 and scoured for more effective ways to make my model better. I decided to address the variety of p-values first, since a variable's p-values give us insight on how influential it may be on our dependent variable (price). One of the really useful coded features I used to select the significant variables based on their p-values for my predictive model was stepwise regression. This method removes or adds variables to our model based on a threshold that we set -- in the case of my model, variables with p-values below 0.05 were added and ones above were dropped. How efficient is that, especially for large data sets with tons of variables! Now that my model only included variables with significant p-values, my adjusted R-squared jumped from 0.57 to 0.70. 

Now that I was happy with the statistics of my model, I had to check how accurate it was by finding its mean squared error (MSE). And then I got scared. 

My MSE was in the 5 trillions! 

Boy, was I about to end it all. I referred back to the examples within the model and saw that their MSE was below 5. 5! Not 5 trillion. Of course, I had to google "What is a good MSE?" and I found a great post on (you guessed it) StackOverflow that eased my worries. Guys, it's all relative. I forgot to compare it to the range of my dependent variable. It also completely left my mind that the although the MSE is often used to see how accurate your model is, it is in square units of the dependent variable. Square rooting my MSE gave me my RMSE in dollars (not dollars squared), which was around 200,000 and was in the range of my house prices. Phew. Still... pretty high, but not national debt high. That's good! 

**Lesson 3**: Everything is relative. Everything!

Anyway, thanks for reading some post insights on one of the first big bosses I encountered on my journey towards data science.  Despite the frustration and ineptness I felt during the whole process, I revel in the fact that I pushed on. And I'll continue to do so. 

Now for beer, Pathfinder and -- geez, do I need nap. 
























