# Chapter 17: Build Prediction Models #

Computational tools help us to automate away the drudgery involved in our work, and they do this best when this work contains automatable patterns. If we can recognize them, we can build a script to repeatedly perform them. This is what we did in Chapter 2, when we hand-built a script to grab the dialogue in a story, and in Chapter 13, when we used the regular-expression library to describe the more complicated patterns we saw in the Python interpreter's error messages and rewrite them into a form easier to digest.

*Pattern matching* is a broadly applicable technique, and in this chapter, I'll introduce you to one of its most powerful uses: the building of predictive models. This returns us to the data science process introduced in Chapter 8, where we covered its first steps (i.e., data collection, data cleaning, and data exploration). I promised you that we'd return to cover its last steps (i.e., pattern detection and model building), and that time is now.

However, unlike earlier chapters, this time we won't have to identify the patterns we wish to exploit. Because data scientists often work with data sets that are too large for humans to analyze, they need tools that can help to identify and exploit the patterns in these data sets. These tools exist in a branch of artificial intelligence called *machine learning (ML)*, and we'll look at one popular library for it called scikit-learn. With such a library, it is very easy to mine a data set for patterns and create a model that uses the patterns it learned to make predictions about new data. In effect, we're asking the computer to analyze a data set and build an algorithm that we would find too difficult to build for ourselves.

We'll explore two real-world problems in this space: (1) the predicting of housing prices based on a home's characteristics (e.g., its number of bedrooms and bathrooms); and (2) the labeling of online comments as toxic. The first is a classic and straightforward ML application. We'll use it as an introduction to ML tools and techniques. The second is a more difficult problem, and it will open our eyes to the dangers of bias in the ML models we build. I'll mention two of the many ways in which bias can creep into a model so that you can begin checking your own. Unfortunately, eliminating bias remains a hard problem, and sometimes, when the harms a model creates are greater than its benefits, you simply shouldn't deploy it.

```{admonition} Learning Outcomes
Learn to use ML as a tool for discovering and exploiting real-world correlations. You will perform supervised learning to build a decision-tree model that that predicts home prices. You'll practice with pandas and data frames (i.e., table data structures). In addition to a focus on predictive accuracy, you'll grapple with questions of bias in ML. After completing this chapter, you will be able to do the following:

*   Explain how ML works and when large data sets are required to build a good predictive model [design and CS concepts].
*   Use kaggle.com to find real-world data sets and example ML models [programming skills].
*   Describe the difference between supervised and unsupervised learning [CS concepts].
*   Work with the popular pandas and scikit-learn libraries to explore large data sets and create ML models [programming skills].
*   Validate the predictive accuracy of a ML model and quantify it using the mean absolute error metric [CS concepts and programming skills].
*   Decide when a ML model is appropriate to use and discuss some of the types of bias it might contain [design and CS concepts].
```

## Predicting home prices

Imagine that your sister is a realtor in Iowa, and she's found that she's drowning each month in data about the housing market. This sounds like a problem that could be solved with computational thinking and a bit of programming, and so you say, "Don't worry, sis! I've practiced computational thinking and problem solving. Send me the data, and I'll build you a computational model based on past home sales. When a new seller calls you, just ask them to tell you about a few features of their home, feed that information into the model, and it will instantly spit out a market-appropriate price for their home." She breaks into a huge smile and says that you're now her favorite sibling.

This is our first problem-to-be-solved, and it has three pieces:

1. building a computational model that considers a home's features and uses them to predict a selling price; 
2. tuning the model to produce good predictions; and
3. using the tuned model to predict the prices of previously unseen homes in a business setting.

We'll focus on the first two pieces, and your sister will use our tuned model in her business.

## Your sister's data

Let's assume that your sister lives in Ames, Iowa. In 2011, Dean De Cock published a data set describing the housing sales in this area from 2006 to 2010.[^fn1] He published it for use in undergraduate regression courses, and it has since become a popular data set for learning about ML. We'll grab a copy of this data set from the online platform kaggle.com, where you can join a community of ML enthusiasts and peruse a repository of community-published data sets and ML models.[^fn2]

The Ames-Iowa-Housing data is one large CSV file. Recall that a CSV is a text file that contains row after row of comma-separated values. It is tabular data that you could read in a spreadsheet application, but let's practice our Python and write a small script (`head.py`) to look at the first five rows of this data set.

```{code-block} python
---
lineno-start: 1
---
### chap17/head.py
import sys, csv

def main():
    if len(sys.argv) != 2:
        sys.exit('Usage: head.py input.csv')
    
    with open(sys.argv[1], encoding='utf-8') as fin:
        reader = csv.DictReader(fin)  # file has a header row
        for i, row in enumerate(reader):
            # Just print the first 5 rows
            if i < 5:
                print(row)
            else:
                break

if __name__ == '__main__':
    main()
```

This script uses the `csv` library and its `DictReader` function, which takes a table's column headings and uses them as dictionary keys. When you run `head.py` on your copy of the Ames-Iowa-Housing CSV file, you should see the first five data rows printed as dictionaries. Each row is a home sale, which lists a lot of information. My copy of the data set contained 82 different pieces of information about each home sale, including `Year Built` and `SalePrice`. We now know why your sister asked for help.

```{admonition} You Try It

Download the Ames-Iowa-Housing data (e.g., from Marco Palermo's Kaggle site[^fn3]) and run `head.py` on the complete CSV file.
```

## Solving this problem ourselves

We don't know much about selling houses, but your sister has said that homes with more bedrooms sell for more money. OK, we know that for making decisions like this, we simply need to use some if-statements. Maybe we can write something like the following, where I've indicated the relationship between the predicted prices but didn't specify their actual values.

```{code-block} python
---
lineno-start: 1
---
### Pseudocode implementing a simple decision tree

# Grab data about new seller's home

# Predict price based on number of bedrooms
# if home.bedrooms > 2:
#     print('Predicted price:', A_BIG_NUMBER)
# else:
#     print('Predicted price:', A_SMALL_NUMBER)
```

To figure out what the predicted prices should be, let's write a Python script that analyzes the Ames-Iowa-Housing data. This script, which I've called `avg_pbbr.py` (i.e., average price by bedroom), buckets the previously sold homes by their number of bedrooms and then calculates and prints each bucket's average sale price. Because it uses a fixed-size array, it is careful to verify that there are no homes in the data set with more than nine bedrooms. It also demonstrates yet another way to do some formatted printing.

```{code-block} python
---
lineno-start: 1
---
### chap17/avg_pbbr.py -- custom-made for Ames-Iowa-Housing data set
import sys, csv

# Each `homes` element is a tuple with these elements:
# 1. count of homes with a number of bedrooms equal to the index
# 2. total sales price for all such homes
MAX_BEDROOMS = 9
homes = [
    (0, 0), (0, 0), (0, 0), (0, 0),
    (0, 0), (0, 0), (0, 0), (0, 0),
    (0, 0), (0, 0),
]

def main():
    if len(sys.argv) != 2:
        sys.exit('Usage: avg_pbbr.py input.csv')
    
    with open(sys.argv[1], encoding='utf-8') as fin:
        reader = csv.DictReader(fin)
        for row in reader:
            # Grab this entry's number of bedrooms
            num_bedrooms = int(row['Bedroom AbvGr'])
            assert num_bedrooms <= MAX_BEDROOMS, f"We need a bigger list ({num_bedrooms})"
            
            # Grab its sales price
            price = int(row['SalePrice'])
            
            # Update the right tuple in homes
            homes[num_bedrooms] = (
                homes[num_bedrooms][0] + 1,
                homes[num_bedrooms][1] + price
            )
        
        # Print out the results
        for i, t in enumerate(homes):
            if t[0] != 0:
                print(f'Avg. sale price of {t[0]:>{4}} homes with {i} bedrooms: ', end='$')
                print(format(t[1]/t[0], '.2f'))

if __name__ == '__main__':
    main()
```

```{admonition} You Try It
Run `avg_pbbr.py` on the CSV file you previously downloaded, and you'll see that there's no observable pattern in the printed averages.
```

Perhaps a pattern will emerge if we include additional home features, like lot size. We'd extend our earlier pseudocode as follows:

```{code-block} python
---
lineno-start: 1
---
### Pseudocode with a larger decision tree

# Grab data about new seller's home

# Predict price based on number of bedrooms and lot size
# if home.bedrooms > 2:
#     if home.lotarea > 12000: 
#         print('Predicted price:', A_BIG_NUMBER)
#     else:
#         print('Predicted price:', A_NOT_AS_BIG_NUMBER)
# else:
#     if home.lotarea > 12000: 
#         print('Predicted price:', A_SMALL_NUMBER)
#     else:
#         print('Predicted price:', A_SMALLER_NUMBER)
```

```{figure} images/Smith_fig_17-01.png
:name: c17_fig1_ref

Decision tree for predicting the price of a home based on the number of bedrooms and lot area. A home with many bedrooms and a large lot is expected to sell for a large amount.
```

What we're building is called a *decision tree*, which I've illustrated in {numref}`Figure %s <c17_fig1_ref>`. For each characteristic we think is important in determining the predicted price, we include a node in the tree (i.e., an if-statement) that directs us toward a subtree (i.e., subsequent if-statements) where that characteristic is true. When we get to the leaves of the tree (i.e., when we run out of conditions to check), we print the predicted price for a house with those characteristics. In our pseudocode, for instance, `A_SMALL_NUMBER` is the predicted price for a home with two or fewer bedrooms on a lot of more than 12,000 square feet.

Unfortunately, even if we were patient enough to figure out what buckets of lot sizes to use, we'd still find that this decision tree doesn't use enough of the data to create a good predictor. We need a smarter approach. We need *machine learning*.

## Machine learning

We've been trying to suss out a pattern in the Ames-Iowa-Housing data that we could use as the foundation for a good predictor, and we were using your sister's experience in real estate as a starting point in this search. This wasn't a bad idea since evolution has made humans into good pattern recognizers---as we discussed in Chapter 1, the accumulation of experience turns good programmers into great ones, who seem to quickly jump to the "right" structure for a script to solve a new problem. But we don't have your sister's experience, and the patterns in the Ames-Iowa-Housing data might be quite subtle. ML is a computational approach that enables computers, using *statistical techniques* and *a large training data set*, to build *models* that perform like an experienced human. They allow all of us to become instant experts in a new domain.

The statistical technique we used a moment ago was a decision tree. Statisticians recommend this approach when the relationship between the *features* (e.g., the characteristics of a home such as its number of bedrooms and lot size) and the *target variable* (e.g., the home's selling price) is complex. For those of you that know a bit about statistical analysis, this often means that the relationship is nonlinear or includes important outliers.

Decision trees produce fine predictors of home prices, and yet there are other statistical techniques that yield more accurate predictions. Despite this, we'll continue using decision trees. They are a great starting place for learning how to do ML because they are easy to understand and form the basic building block of several better techniques.

## Labeled training data

I said that ML relies on statistical techniques and a large training data set. We have been using the prices for past home sales as accurate indicators of the prices of future home sales. In other words, these past home sales are the experience we need to predict future home sales, and we can therefore use these past home sales to *train* our ML model. You'll often see this described as *fitting* a model to the training data.

When there is a strong correlation between a feature (e.g., number of bedrooms) and the target variable (e.g., selling price), we might need to see very few training examples to build a good prediction model. However, we learned using `avg_pbbr.py` that there isn't a strong correlation between the number of bedrooms and selling price in the Ames-Iowa-Housing data. But since the real estate industry uses comparable houses to set prices, there must be some correlation between the features of a home and its sale price. We simply need a large enough data set to discover that correlation.

```{tip}
When the patterns are hard to see, you'll need a large training set to have ML create a good predictor.
```

There's one other characteristic of the Ames-Iowa-Housing data that doesn't have to be true in every ML problem: these data are *labeled*. This term means that the data set includes the answers (i.e., the value of the *target variable*, which for our problem is the selling price). In other words, we know not only the values of the features of each home, but we also know its true selling price. When using labeled data, the process is called *supervised learning*.

But not all ML requires labeled data. When we want to analyze a data set for any kind of pattern, it is called *unsupervised learning*. We won't talk further about this type of ML except to say that you should investigate it if you're interested in problems such as natural language processing, object recognition, customer segmentation, or exploratory data analysis.

```{tip}
The discipline of ML includes a dizzying number of statistical techniques under the headings of supervised and unsupervised learning. As you work in this area, you'll learn that there is no single best method, and you'll find that no one can accurately predict whether a particular approach will be successful. Experience will help limit the amount of trial and error you'll do, but be prepared to try lots of different approaches until you find one that works well.
```

## ML workflow

We're about ready to dig into a ML library and solve our sister's problem, but before we discuss its particulars, I want to describe in general how we're moving work off our plates and on to the machine's. At the start of the chapter, we tried to follow a process like this:

1. We looked at the data and picked one or more features we thought might correlate well with the target variable.
2. We wrote a script (`avg_pbbr.py`) that analyzed the data in the data set to see if a pattern existed between our chosen features and the target variable. If not, we returned to step 1.
3. Using what we learned from the analysis script, we would have filled in the unknown numbers in our decision-tree pseudocode and written a script that takes as input a set of feature values and outputs a predicted target value. This would have been the "model" we would have sent to your sister.

In using this model, your sister probably would find that it didn't do an outstanding job of predicting the selling price of a new home coming on the market. This is because the steps we took didn't involve looking for the *best* model we could have built; we simply built *a* model based on the first pattern we found. An experienced data scientist would iterate and ask, "OK, my first model works, but is there a better selection of features (or even a superior statistical approach) that produces a better predictor?"

A good ML library automates much of steps 2 and 3 for you. We'll use the `pandas` and scikit-learn libraries, and with their help, this process turns into the following:

1. Review and analyze our data set---we'll use the `pandas` library for this work---and pick one or more features we think might correlate well with the target variable.[^fn4]
2. Select a type of statistical analysis that'll lie at the heart of our model. We'll use the `DecisionTreeRegressor` class in scikit-learn. Besides decision trees, this library supports many other statistical techniques, and we'll employ a different one when we later consider a predictor for the toxicity of online comments.
3. Create an instance of the `DecisionTreeRegressor` class and call the resulting object's `fit` method, giving it the set of features and the target variable we want it to use from our training data.
4. Test this object, which is a model fitted to our training data using supervised learning, against some previously unseen data, and evaluate whether this model is a good one. If it is not, we'll return to an earlier step, changing perhaps the features included or the type of statistical analysis performed. 
5. Share our best model with your sister. 

Notice that we no longer need to write the code that implements a statistical analysis (step 2) or the model that fits the training data (step 3), as we tried to do earlier. Using powerful libraries like `pandas` and scikit-learn, our scripts need to make only a few function calls.

## Getting a feel for the data

With this foundation, you are ready to start using the tools of a data scientist. We'll begin with the `pandas` library, which "is a fast, powerful, flexible and easy to use open source data analysis and manipulation tool, built on top of the Python programming language."[^fn5] It provides data structures and functions that allow us to quickly explore a data set.

```{tip}
Many data scientists work in interactive Python notebooks (`ipynb` files) when doing ML, and we'll do the same in the rest of this chapter. They also commonly refer to the `pandas` library with the abbreviation `pd`.
```

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 4th code block
import pandas as pd
```

The most important data structure in the `pandas` library is the `DataFrame`. You can think of a `DataFrame` as resembling a table, like a worksheet in a spreadsheet application. The `pandas` library provides input/output functions, like `pandas.read_csv`, that allow you to pour data into and pull data out of a `DataFrame`. And once your data is in a `DataFrame`, you can apply many of the library's powerful functions, such as `pandas.DataFrame.describe` that creates a summary description of the data in each column. The following code block shows how we can use the `pandas` library to begin exploring the data set containing home sales in Ames, Iowa.

```{admonition} Terminology
:class: tip
Each column in a `DataFrame` is a `pandas.Series` data structure, and data scientists will often refer to a column as a series.
```

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 3rd and 5th code blocks
# Specify the input CSV file for building and testing the model
csv_file = 'AmesIowaHousingData/AmesHousing.csv'

# Read the CSV data into a DataFrame
df = pd.read_csv(csv_file)

# Print a summary of the data
df.describe()
```

``` {admonition} You Try It
Set the variable `csv_file` to the location of where you placed the Ames-Iowa-Housing data set. Execute the referenced code blocks in `ames.ipynb` as you read through this chapter. Remember that the interactive Python interpreter will print the result of the last statement in a code block when you execute it in an `ipynb` file. You'll need to see the returned result for `df.describe()` to follow the explanation that comes next.
```

The results of our call to `df.describe()` list eight numbers for many but not all of our data set's columns. The `pandas` library decided that each of these columns is a *numeric* series, and the numbers under the series name tell you some statistical facts about each of these columns. Here's a short summary of the meaning of each row in the table returned by `describe`:

* `count` reports the number of rows (in the indicated column from the data set) with *non-missing values*. As we discussed in Chapter 8, there are many reasons why a data set may contain missing values. In our data set, for example, two of the home sales don't report any value for `Bsmt Full Bath`, which might have occurred because the person filling out the data didn't think that they needed to fill out this column for a house without a basement.
* `mean` is the arithmetic average of the non-missing values.
* `std` is the standard deviation of the non-missing values, and it provides a measure of the values' numerical spread.
* `min`, `25%`, `50%`, `75%` and `max` are best considered together. If you were to sort the non-missing values in the column from the smallest number to the largest, the first is `min` and the last is `max`. Then imagine drawing three lines that split the sorted list into four equal-sized buckets. The numbers at these three split points are the values at the 25th percentile (`25%`), the 50th percentile (`50%`), and the 75th percentile (`75%`). Intuitively, the 25th percentile is the number that is bigger than a quarter of the column's values and smaller than the other three-quarters.

Knowing this, I can see that my copy of the Ames-Iowa-Housing data contains 2,930 entries, and the average home sale price between 2006 and 2010 was approximately 180,800 dollars, with the cheapest home selling for just under 80,000 dollars and the most expensive one for 755,000 dollars.

The `pandas` library limits the number of `DataFrame` columns and rows it displays, and if we wanted to see the statistics about bedrooms above ground (`Bedroom AbvGr`) that we analyzed with our own script earlier, we'd either have to change the maximum number of columns that `pandas` displays or slice that series from the result of the `describe` method call. Let's choose the latter approach:

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 7th code block
# Review only the statistics for number of bedrooms
df.describe()['Bedroom AbvGr']
```

From this result, we can see that two- and three-bedroom houses are common, and that we didn't need to worry about tracking houses with more than eight bedrooms.

## Set the prediction target

We're now ready to define the setup we'll use in configuring our prediction model. Let's start with the model's output, which is the variable we want to predict. The series labeled `SalePrice` is this target variable, and by convention, data scientists call it `y`.

To set what `y` names, we can grab that series from our `DataFrame` using one of two notations: (1) square brackets to slice it out, as we've done with other sequences; or (2) what's called *dot notation* in `pandas`. The former always works, while the latter is a nice shorthand when the series name doesn't contain spaces or conflict with another attribute name in the `pandas` library. The following code block illustrates both methods while using the latter since this label meets the requirements for using the shorthand.

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 8th code block
# Setting the prediction target
# y = df['SalePrice']
y = df.SalePrice
y
```

## Pick some features

As we did when we tried to solve this problem by writing our own code, we need to choose which features we think will correlate nicely with our target variable. Almost every series is a candidate except the first, which in my copy of the data set is called `Order`, and the last, which is the target variable. I eliminate the first series from contention since it is nothing more than each record's index in the data set. You'd want to eliminate anything like it when it's clear that series could not possibly correlate with the target variable.

To choose among the others, let's interrogate the `columns` attribute on a `DataFrame`, which prints all the column labels.

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 9th code block
df.columns
```

As you'll find in many big data sets, there are a lot of columns to consider. While it might be tempting to include all the eligible columns in your model, that's not always the best choice. Let's begin with just a few; later, we can always later change the list of features included and compare the performance of different models.

In the next code block, we build a list of column names that represent a collection of features we want included in our model. We then use this list to slice those series out of our `DataFrame`. Again, by convention, these data are called `X`. This and the following code block invoke the `describe` and `head` methods on the `DataFrame` called `X` to check what we've done.

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 10th code block
# The model's input features
feature_names = ["Lot Area", "Year Built", "1st Flr SF", "2nd Flr SF", "Full Bath", "Bedroom AbvGr", "TotRms AbvGrd"]
X = df[feature_names]
X.describe()
```

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 11th code block
X.head()
```

```{tip}
As we've learned, frequent checks of our script's state help us to find mistakes in our logic. These sorts of checks are even more important in the work of a data scientist, since it is harder to spot a model built from nonsensical data. Check the contents of your data frames frequently!
```

## Fit the model to our data

We chose to use a decision tree, which the scikit-learn library[^fn6] provides through the `DecisionTreeRegressor` class. Instantiating an instance of this class is the first step in creating a model that uses decision trees.

Hidden in this creation step, however, is a bit of randomness, which many ML libraries use as a best practice for creating robust models. Since we want to compare the models we build against each other, we want to eliminate this randomness as a potential cause of any difference in the performance of our models. We can accomplish this by specifying the random seed that the model should use each time it creates a new instance for us. This is the purpose of the `random_state` assignment in the constructor call in the following code block. You can choose any number you'd like as long as you consistently use it in all your constructor calls.

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 12th code block
from sklearn.tree import DecisionTreeRegressor

# Create an untrained model
my_model = DecisionTreeRegressor(random_state=42)

# Fit the model to the specified portion of the training data
my_model.fit(X, y)
```

Given a newly created instance of `DecisionTreeRegressor`, we simply pass our `X` and `y` variables to this object's `fit` method, and it updates the model. That was easy! We'll now use this object to make predictions.

## Predicting unseen data

To make a prediction using this fitted model, we need a set of values for the model's input features. Inputting these feature values will cause the model to return a prediction for the target variable. In other words, have someone tell us about a house we haven't yet seen and, for that house, give us its lot size, the year it was built, the square feet of living space in the first and second floors, and the number of full bathrooms, bedrooms above ground, and total rooms above ground. That's the list of features we used to train our model to predict selling prices.

OK, but we used all the data we had about home sales in Ames, Iowa, to create the model. We could go back to your sister for more data, but there's another way: data scientists take a given data set and split it in two. One part of this split is designated the *training set* and the other is the *test (or testing) set*. We'll use the former to create our model and the latter to check its performance.

There are many ways to poorly split a data set. To avoid these pitfalls, the train-test split routines in ML libraries like scikit-learn employ randomness. Again, we want to control the random seed used by these functions so that we can ensure that all the models we build are fed the same training and testing data sets.

The code block below retrains our model using the `train_X` and `train_y` data sets produced by the `train_test_split` function in the scikit-learn library. It then illustrates how to use the `predict` method on our model object, into which we feed the `test_X` data set. The resulting `predictions` are what we want to compare against the actual sale prices stored in `test_y`. I've written a little loop that does this comparison for the first five predictions, converting a few data types along the way (i.e., the `Series` in `test_y` into a Python list and each `float` in `predictions` into an `int`).

```{tip}

Don't test with your training data. A model's utility is defined by its ability to make accurate predictions on data it hasn't previously seen, which are called the *validation data*. It's not hard to get good predictions from a model's training data.[^fn7]

```

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 13th code block
from sklearn.model_selection import train_test_split

# Split features and target data into training and validation sets
train_X, test_X, train_y, test_y = train_test_split(X, y, random_state=42)

# Fit the model using the training data
my_model = DecisionTreeRegressor(random_state=42)
my_model.fit(train_X, train_y)

# Feed the model the test data and capture the resulting predictions
predictions = my_model.predict(test_X)

# Compare first 5 predictions against actual sale prices
actuals = test_y.to_list()
for i in range(5):
    p = int(predictions[i])
    a = actuals[i]
    d = p - a
    print(f'prediction = ${p}; actual = ${a}; diff = ${d:>6}')
```

```{admonition} You Try It
Run the code above. Are you happy with the resulting predictions? Did we build a good model for your sister?
```

## Model validation

*Predictive accuracy* is the first measure that data scientists consider when deciding whether a model is good. If a model correctly predicts the target variable a sizable percentage of the time, they say it has good predictive accuracy. If it doesn't, they throw the model away and try again.

You're probably thinking: How often does the model need to be correct---that is, what percentage classifies as "sizable"? The answer depends on how you'll use the model. For your sister's use case, her reputation will be damaged if the sale prices she suggests to her clients are far from what other experienced realtors suggest. But being slightly off the actual sales price isn't a big deal; no one expects a home to sell for exactly its listing price. This means that we want a reasonably high predictive accuracy---for example, 70−90 percent of the time the model predicts the selling price within a couple of thousand dollars.[^fn8] On the other hand, you probably wouldn't be happy with a predictive accuracy of 9 in 10 if your doctor was using the model to decide whether your X-ray showed a tumor.

OK, but how do we check if the model we built meets this 7-to-9-in-10 target? When I ran the previous code block, which compared the first five predicted home prices against the actual prices these homes sold for, it printed the following:

```{code-block} none
---
lineno-start: 1
---
prediction = $231000; actual = $161000; diff = $ 70000
prediction = $100500; actual = $116000; diff = $-15500
prediction = $193000; actual = $196500; diff = $ -3500
prediction = $138000; actual = $123600; diff = $ 14400
prediction = $103000; actual = $126000; diff = $-23000
```

Not one of the predictions matched the actual selling price; the model made errors on both sides of the actual price; and the first prediction is particularly bad. As humans, we have a hard time digesting just these five results, and it will be impossible for us to mentally process all the differences in the full testing set. We need a single metric (or no more than a few metrics) that the computer can compute for us that will indicate how well our model did overall.

In practice, there are many such metrics, and we'll use one of the most popular, which is called the *mean absolute error (MAE)*. Intuitively, MAE indicates how far off, on average, the model's predictions are from the actual values. While we might be interested in knowing if the model is always wrong in a particular direction (e.g., the predicted value is always larger than the actual), this metric throws away that information by first taking the absolute value of the difference between each predicted and actual value. Then, from these magnitudes, it computes a simple arithmetic average and reports that as the model's MAE on the test set. The scikit-learn library provides a function that computes MAE:

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 14th code block
from sklearn.metrics import mean_absolute_error

test_mae = mean_absolute_error(test_y, predictions)
print(f'MAE = ${int(test_mae)}')

# Compare the MAE against the average home price
test_mean = test_y.describe()['mean']
print(f'Mean price = ${int(test_mean)}')
print(f'Percentage of price = {int(100 * test_mae / test_mean)}%')
```

Our first attempt at building a model for your sister produced one with a MAE that's about 16 percent of the test set's average home price. Not great, but not too bad for our first try!

## Making the fit just right

As we discussed earlier, we don't want to send the first model we get working to your sister. We want to vary how we build the model and send her the best one. If you continue in data science, you'll quickly learn that there are many choices you make in building a model, and changing these choices affects the model's quality. Some of these choices are absolutes, like "I'm going to build a decision-tree model." Others are like knobs or sliders, and you can use a little of that thing to a lot of it. In this latter category of choices, what's important is that you're looking for an amount of the thing that is just right. Technically speaking, the just-right amount creates a model that is neither *overfitted* nor *underfitted*.

Let's explore this idea with the `max_leaf_nodes` parameter to the `DecisionTreeRegressor` class, which is slider-like. This parameter limits the number of leaves the scikit-learn library can create in our decision tree, which you can think of as limiting the number of distinct house types in the decision tree. Your first thought might be, "Why would I want to limit the number of distinct house types? Wouldn't more be better?" Well, let's see!

The next code block runs a simple experiment: It builds seven decision-tree models, where each model is built using a different limit on the maximum number of leaf nodes that the library can create. Each model is trained and tested on the data set split from earlier. The code prints each model's MAE, and when complete, it prints the value of `max_leaf_nodes` that produced the lowest MAE.

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 15th code block
# Find the best model by varying the size of the decision tree
best_num_leaves = 0
lowest_mae = 9999999.9

# Run the experiment
print('Leaves\tMAE')
for leaves in [4, 16, 64, 128, 1024, 16384, 131072]:
    my_model = DecisionTreeRegressor(max_leaf_nodes=leaves, random_state=42)
    my_model.fit(train_X, train_y)
    predictions = my_model.predict(test_X)
    test_mae = mean_absolute_error(test_y, predictions)
    print(f'{leaves}\t${int(test_mae)}')
    
    if test_mae < lowest_mae:
        # Update best
        best_num_leaves = leaves
        lowest_mae = test_mae

# Report best
print(f'\nBEST model uses {best_num_leaves} leaves')
```

```{admonition} You Try It
Before you run the code block above, guess which of the bounds (i.e., 4, 16, 64, 128, 1024, 16384, or 131072) will produce the model with the lowest MAE. Then run the code and check to see if you were right.
```

The experiment illustrates a common phenomenon: 

* Too little of a thing, like leaves in a decision tree, produces a poor model. This probably makes sense to you because too few categories will throw very different houses together in the same average. We saw this earlier when we tried to predict a price solely based on the number of bedrooms in a home. Technically, the model underfits the training data; it doesn't capture the patterns in the data set important to predicting the target variable. 
* Too much of a thing, perhaps paradoxically, also produces a poor model. The problem this time isn't that the model didn't capture the important patterns. It did. The problem is that it captured too many patterns, and in particular, the model is paying attention to patterns that exist only in the training data and not in the larger world. Technically, the model overfits the training data.

Your best model will sit in the sweet spot between underfitting and overfitting. Starting with little of a thing, the MAE will decrease as you add more of that thing. At some point, however, as you continue to add more, the MAE will start to increase. With the split I created in my version of the Ames-Iowa-Housing data, the MAE was over 37,000 dollars with 4 leaves on the decision tree, and the MAE decreased as I increased `max_leaf_nodes`. However, it started increasing again at 128 leaves. My experiment's best outcome for `max_leaf_nodes` was 64, producing a MAE that was 14 percent of the test set's average home price. Your sister will be happier with this model than our first!

```{tip}
We lowered the MAE, but we haven't necessarily found the best model. The last code block in `ames.ipynb` uses the `RandomForestRegressor` to create a model with an even lower MAE. You stop your search for lower MAEs when you have a model whose prediction accuracy is "good enough" for your problem domain.
```

## Bias in ML

In terms of predictive accuracy, we've essentially solved your sister's problem. But this metric isn't the only one that matters. What do you think would happen if I called your sister and asked her to use the ML model you built to predict the selling price of my home in Boston? In other words, should a Boston homeowner trust the model's output? And if not, why not?

Being a good real estate agent, your sister would know that she can't help me. When I ask her my question, she'd almost certainly say, "My knowledge of home prices is specific to Ames, Iowa. Two identical houses, one in Ames and the other in Boston, will sell for different prices.[^fn9] I can't help you." And neither can your model, which like your sister, was trained exclusively on data specific to Ames, Iowa.

Data scientists would say that our model exhibits *representation bias*, which means that the data set we used to train it did not represent the area in which I live. This is only one kind of bias that can creep into our ML work, and Harini Suresh and John Guttag published an excellent paper[^fn10] that details six types of ML biases, including this one. Because ML is increasingly influencing important decisions in our lives, like the selling cost of our homes, we need to understand these different kinds of biases and evaluate our ML models for them.

Even though the model we built exhibits representation bias, it's still a good model if we understand when we should and shouldn't use it, as we just discussed. On the other hand, a model may contain biases that make it useless for its intended purpose, and if you're solely focused on your model's predictive accuracy, you may completely miss this and ship a model that does real harm to individuals and society. Let's look at an instance of this with the help of Alexis Cook, who has written a tutorial on Kaggle titled "Identifying Bias in AI."[^fn11]

The type of bias that she explores in the exercise at the end of her tutorial is called *historical bias*. The problem here is not that a training data set doesn't represent the world in which we want to make predictions (i.e., representation bias), but that that world exhibits patterns we don't want to use as the basis for future predictions.

## Classifying comments as toxic

Cook's exercise uses part of a data set containing approximately 2 million public comments collected from a range of online news sites that used a civility plugin produced by a now-defunct company called Civil Comments.[^fn12] When the company shut down, it released this data set, which was picked up by Alphabet's Conversation AI team and turned into a 2019 Kaggle competition.[^fn13] The goal of the competition was to build a ML model that could identify which comments in this data set were toxic and do so without discriminating against individuals because of their age, race, gender, religion, or other legally protected characteristics. This problem is hard because discrimination exists in our world, and it is reflected in many of the data sets we collect. We want a model that identifies toxic comments without also learning our society's patterns of historical discrimination against particular individuals.

Cook steps you through the building of a `LogisticRegression` model, which takes a comment as a string and classifies it as toxic or not. You can check out the ML code in Cook's tutorial.[^fn14] My goal here is to help you focus on what makes identifying historical bias hard so that you aren't caught unaware when you build your own ML models.

From her snapshot of the entire data set, Cook's code pulls two example comments that illustrate toxic and not-toxic comments. I repeat below these two labeled comments:

```{code-block} none
---
lineno-start: 1
---
Sample toxic comment: Too dumb to even answer.
Sample not-toxic comment: No they aren't.
```

Using supervised learning, Cook then fits a model to her training data, and she shows that this model achieves a predictive accuracy of 93 percent on the test data. This is a high level of accuracy, and if we stopped evaluating the model after viewing this metric, we'd be in trouble.

```{tip}
Check for bias even if your model's predictive accuracy is good. This chapter is about patterns, and don't be lulled into complacency by thinking that a strong predictive accuracy is a sign of a good ML model. Representation bias often inversely correlates with predictive accuracy, but historical bias does not.
```

Cook's tutorial then encourages you to classify these four comments using the model: 

1. "I have a white friend"
2. "I have a black friend"
3. "I have a christian friend"
4. "I have a muslim friend"

We know that none of these should be classified as toxic comments; yet the model classifies the first and third as not-toxic and the second and fourth as toxic.

What's going on? Well, the world has historically discriminated against individuals because of their race and their religion. We know this, and if you look at the comments in the data set marked as toxic, they often involve discriminatory comments against Blacks and Muslims. The model classifies our second and fourth comments as toxic not because these words alone are toxic, but because they were often included in comments labeled as toxic. The model learned a discriminatory pattern and assumed it was important in classifying comments as toxic.

The result is a model that exacerbates the historical discrimination against a segment of our society. Despite its strong predictive accuracy on the test set, we should not deploy it.

## More art than science

The toxicity-prediction model notwithstanding, data science has the capacity to do good and help people. This chapter is meant to get you started in this exciting field and make you aware of its biggest challenges.

To build a good model, experience and "taste" matter, as we saw in trying to choose which features to include and where we'll place a model's sliders. Because of this, the field remains more art than science. Take advantage of the libraries that aid in this work, and don't get frustrated if your first models don't perform well.

In this chapter, we built fairly good predictors using relatively simple statistical models. The benefit of simple models is that their predictions are generally easy to explain. For example, one house sells for more than another because it contains more bedrooms (everything else being equal), or a comment is considered toxic when it contains words often used in toxic comments. In a quest for ever more accurate predictors, data scientists are regularly developing more complicated models whose predictive performance we cannot as easily explain. Nonetheless, a model's explainability may be important in some application areas. For instance, I generally don't think about how autocorrect comes up with its suggestions while I type my text messages, but predictive models won't likely succeed in clinical medicine unless they are explainable. Do pay attention to what's important in your application area.

Finally, a data scientist's work is not complete until they attach a story to their models and associated discoveries. These stories require creativity, an adherence to the truth of what has been learned, and a thoughtfulness about how this information might be used for good and evil. May you use the knowledge you've gained to make the world a better place for all.

[^fn1]: Dean De Cock, "Ames, Iowa: Alternative to the Boston Housing Data as an End of Semester Regression Project," *Journal of Statistics Education*, 19, no. 3 (2011), https://jse.amstat.org/v19n3/decock.pdf. The copy of the Ames, Iowa housing data that I use in this chapter is from Marco Palermo's Kaggle site (https://www.kaggle.com/datasets/marcopale/housing). As we discussed in Chapter 8, we'd want to inspect and clean a data set before we use it to build a prediction model, but De Cock has already cleaned the data.

[^fn2]: Kaggle homepage, accessed April 5, 2025. https://www.kaggle.com/

[^fn3]: Marco Palermo, "Ames Iowa Housing Data," accessed April 5, 2025. https://www.kaggle.com/datasets/marcopale/housing

[^fn4]: Even the task of choosing the features (step 1) can be partially automated. If you take a ML course, you'll learn about statistical techniques that can automatically prioritize the features in your data set.

[^fn5]: pandas homepage, accessed April 5, 2024. https://pandas.pydata.org/

[^fn6]: The Python module you want to import to use the scikit-learn library is called \`sklearn\`.

[^fn7]: You can try this by changing the code in 13th code block. Call \`predict\` with \`train\_X\` instead of \`test\_X\`; and set \`actuals\` to be \`train\_y.to\_list()\`.

[^fn8]: This dollar amount reflects our knowledge that the average selling price in the Ames-Iowa-Housing data set is approximately 180,000 dollars.

[^fn9]: Real estate apps like Zillow.com often allow you to compare home values in different regions of the United States. I used this feature on Zillow to compare the September 2024 "Zillow Home Value Index" for Ames (262,085 dollars) and Boston (748,710 dollars).

[^fn10]: Harini Suresh and John V. Guttag, "A Framework for Understanding Sources of Harm Throughout the Machine Learning Life Cycle," EAAMO '21: Proceedings of the 1st ACM Conference on Equity and Access in Algorithms, Mechanisms, and Optimization, art. no. 17 (2021): 1−9. https://doi.org/10.1145/3465416.3483305

[^fn11]: Alexis Cook, "Identifying Bias in AI," accessed April 5, 2025. https://www.kaggle.com/code/alexisbcook/identifying-bias-in-ai/tutorial

[^fn12]: Aja Bogdanoff, "Saying Goodbye to Civil Comments," Medium article, December 21, 2017. https://medium.com/@aja\_15265/saying-goodbye-to-civil-comments-41859d3a2b1d

[^fn13]: cjadams, Daniel Borkan, inversion, Jeffrey Sorensen, Lucas Dixon, Lucy Vasserman, and nithum, "Jigsaw Unintended Bias in Toxicity Classification," 2019. https://www.kaggle.com/c/jigsaw-unintended-bias-in-toxicity-classification

[^fn14]: Alexis Cook, "Exercise: Identifying Bias in AI," accessed April 5, 2025. https://www.kaggle.com/code/alexisbcook/exercise-identifying-bias-in-ai/notebook