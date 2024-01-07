# Chapter 17: Build Prediction Models (Incomplete) #

Computational tools help us to automate away the drudgery involved in our work. They do this best when there are clear patterns in the work we do. If we can recognize them, we can build a script to repeatedly perform the work associated with each pattern. This is what we did in Chapter 2, when we hand-built a script to grab the dialogue in a story, and Chapter 13, when we used the regular-expression library to describe the more complicated patterns we saw in the Python interpreter's error messages and rewrite them into a form easier for us to digest.

Pattern matching is a broadly applicable technique, and in this chapter, I'll introduce you to one of its most powerful uses: the building of predictive models. This returns us to the data science process introduced in Chapter 8, where we covered its first steps (i.e., data collection, data cleaning, and data exploration). I promised you that we'd return to cover its last steps (i.e., pattern detection and model building), and that time is now!

However, unlike our earlier work, this time we won't have to identify the patterns we wish to exploit. Because data scientists often work with data sets that are too large for humans to analyze, they need tools that can help to identify and exploit the patterns in these data sets. The design of these tools exist in a branch of artificial intelligence called *machine learning (ML)*, and we'll look at one popular library for it called `scikit-learn`. With such a library, it is very easy to mine a data set for patterns and create a model that uses the patterns it learned to make predictions about new data. In effect, we're asking the computer to analyze a data set and build an algorithm that we would find too difficult to build for ourselves.

We'll explore two real-world problems in this space: (1) the predicting of housing prices based on a home's characteristics (e.g., number of bedrooms and bathrooms); and (2) the labeling of online comments as toxic or not. The first is classical and fairly straightforward ML application. We'll use it as an introduction to ML tools and techniques. The second is a more difficult problem, and it will open our eyes to the dangers of bias in the ML models we build. I'll mention the many ways in which bias can creep into a model so that you can check begin checking your own. Unfortunately, eliminating bias in ML models remains a hard problem, and sometimes, when the harms a model creates are greater than its benefits, you simply shouldn't deploy it. 	

```{admonition} Learning Outcomes
Learn to use machine learning (ML) as a tool for discovering and exploiting correlations. You will perform supervised learning to build a decision-tree model that makes real-world predictions. You'll practice with pandas and data frames, i.e., a table data structure. In addition to a focus on model accuracy, you'll grapple with questions of bias in ML. After completing this chapter, you will be able to:

*   TBW.
```

## Predicting home prices

Imagine that your sister is a realtor in Iowa, and she's found that she's drowning each month in data about the housing market. This sounds like a problem that could be solved with computational thinking and a bit of programming, and so you say, "Don't worry, sis! I've practiced computational thinking and problem solving. Send me the data, and I'll build you a computational model based on past home sales. When a new seller calls you, just ask them to tell you about a few features of their home, feed that information into the model, and it will instantly spit out a market-appropriate price of their home." She breaks into a huge smile and says that you're now her favorite sibling.

This is our first problem to be solved, and it has three pieces:

1. building a computational model that considers the features of a home and uses those features to predict a selling price; 
2. tuning the model to produce good predictions; and
3. using the tuned model to predict the prices of previously unseen homes in a business setting.

We'll focus on the first two pieces, and your sister will use our tuned model in her business.

## Your sister's data

Let's assume that your sister lives in Ames, Iowa. In 2011, Dean De Cock published a data set describing the housing sales in this area from 2006 to 2010. He published it for use in undergraduate regression courses, and it has since become a popular data set for learning about ML. We'll grab a copy of this data set from the online platform [kaggle.com](http://kaggle.com), where you can join a community of ML enthusiasts and peruse a repository of community-published data sets and ML models.

```{margin} References
You can read De Cock's original paper in the [Journal of Statistics Education, Volume 19, Number 3 (2011)](https://jse.amstat.org/v19n3/decock.pdf). The copy of the Ames, Iowa Housing Data I use in this chapter is from [Marco Palermo's Kaggle site](https://www.kaggle.com/datasets/marcopale/housing). Normally, as we discussed in Chapter 8, we'd want to inspect and clean a data set before we try to use it to build a prediction model, but De Cock has already done the cleaning for us.
```

The Ames-Iowa-Housing data is one large CSV file. Recall that a CSV is a text file that contains row after row of comma-separated values. It is tabular data that you could read into a spreadsheet application, but let's practice our Python and write a small script (`head.py`) to look at the first five rows of this data set.

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
        reader = csv.DictReader(fin)
        for i, row in enumerate(reader):
            # Just print the first 5 rows
            if i < 5:
                print(row)
            else:
                break

if __name__ == '__main__':
    main()
```

This script uses the `csv` library and its `DictReader` function, which takes a table's column headings and uses them as dictionary keys. When you run `head.py` on your copy of the Ames-Iowa-Housing CSV file, you should see the first five data rows of the file printed as dictionaries. Each row is a home sale, which lists a lot of information. My copy of the data set contained 82 different pieces of information about each home sale, including `'Year Built'` and `'SalePrice'`. We now know why your sister asked for help.

## Trying to solve this problem ourselves

We don't know much about selling houses, but your sister has said that homes with more bedrooms sell for more money. Ok, we know about making decisions like this: we simply need to use some if-statements. Maybe we can write something like the following, where I've indicated the relationship between the predicted prices but didn't specify their actual values.

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

To figure out what the predicted prices should be, let's write a Python script that analyzes the Ames-Iowa-Housing data. This script, which I've called `avg_pbbr.py` (i.e., average price by bedroom), buckets the previously sold homes by their number of bedrooms and then calculates and prints each bucket's average sale price. Because it uses a fixed size array, it is careful to verify that there are no homes in the data set with more than nine bedrooms. It also demonstrates yet another way to do some formatted printing.

```{code-block} python
---
lineno-start: 1
---
### chap17/avg_pbbr.py
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

            # Update the right tuple in `bedrooms`
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
Run `avg_pbbr.py`, and you'll see that there's no observable pattern in the printed averages.
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

What we're building is called a *decision tree*. For each characteristic we think is important in determining the predicted price, we include a node in the tree (i.e., an if-statement) that directs us toward a subtree (i.e., subsequent if-statements) where that characteristic is true. When we get to the leaves of the tree (i.e., we run out of conditions to check), we print the predicted price for a house with those characteristics. In our pseudocode, for instance, `A_SMALL_NUMBER` is the predicted price for a home with two or fewer bedrooms on a lot of more than 12,000 square feet.

Unfortunately, even if we were patient enough to figure out what buckets of lot sizes to use, we'd still find that this decision tree doesn't use enough of the data to create a good predictor. We need to a smarter approach. We need *machine learning*.

## Machine learning

We've been trying to suss out a pattern in the Ames-Iowa-Housing data that we could use as the foundation for a good predictor, and we were using your sister's experience in real estate as a starting point in this search. This wasn't a bad idea since we know that evolution has made humans into good pattern recognizers. For example, as we discussed in Chapter 1, the accumulation of experience turns good programmers into great ones (i.e., individuals that seem to quickly jump to the "right" structure for a script to solve a new problem). But we don't have your sister's experience and the patterns in the Ames-Iowa-Housing data might be quite subtle. ML is a computational approach that enables computers, through the use of *statistical techniques* and *a large training data set*, to build *models* that perform like an experienced human. They allow everyone of us to become instant experts in a new domain!

The statistical technique we used a moment ago was a decision tree. Statisticians recommend this approach when the relationship between the *features* (i.e., the characteristics of a home such as its number of bedrooms and lot size) and the *target variable* (i.e., the home's selling price) is complex. For those of you that know a bit about statistical analysis, this often means that the relationship is non-linear or includes important outliers.

Decision trees produce fine predictors of home prices, and yet there are other statistical techniques that yield more accurate predictions (e.g., see ALE 17.??). Despite this, we'll continue using decision trees, since they are a great starting place for learning how to do ML because they are: (1) easy to understand; and (2) they form the basic building block of several better techniques.

## Labeled training data

I said that ML relies on statistical techniques and a *large training* data set. We have been using the prices for past home sales in Ames, Iowa as accurate indicators of the prices of future home sales in that area. In other words, these past home sales are the experience we need to predict future home sales, and we can therefore use these past home sales to *train* our ML model. You'll often see this described as *fitting* the model to the training data.

When there is a strong correlation between a feature (e.g., number of bedrooms) and the target variable (e.g., selling price), we might need to see very few training examples to build a good prediction model. However, we learned using `avg_pbbr.py` that there isn't a strong correlation between the number of bedrooms and selling price in the Ames-Iowa-Housing data. But since the real estate industry uses comparable houses to set asking prices, there must be some correlation between the features of a home and its sale price. We simply need a large enough data set to discover that correlation, to identify the patterns in the data.

```{tip}
When the patterns are hard to see, you'll need a large training set to have ML create a good predictor.
```

There's one other characteristic of the Ames-Iowa-Housing data that doesn't have to be true in every ML problem: this data set is *labeled*. This term means that the data set includes the answers, i.e., the value of the *target variable*, which for our problem is the selling price. In other words, we know not only the values of the features for each of the homes, but we also know their true selling prices. When using labeled training data, it is called *supervised learning*.

But not every type of ML requires labeled training data. When we want to analyze a data set for any kind of pattern, it is called *unsupervised learning*. We won't talk about further about this type of ML except to say that you should investigate it if you're interested in problems such as natural language processing, object recognition, customer segmentation (e.g., recommendation engines), or exploratory data analysis.

```{tip}
The discipline of ML includes a dizzying number of statistical techniques under the headings of supervised and unsupervised learning. As you work in this area, you'll learn that there is no single best method, and you'll find that no one can accurately predict whether a particular approach will be successful. Experience will help limit the amount of trial and error you'll do, but be prepared to try lots of different approaches until you find one that works well.
```

## ML workflow

We're about ready to dig into two ML libraries and solve our sister's problem, but before we discuss the particulars of either, I want to describe in general how we're moving work off our plates and on to the machine's. At the start of the chapter, we tried to follow a process like this:

1. We looked at the data and picked one or more features we thought might correlate well with the target variable.
2. We wrote a script (e.g., `avg_pbbr.py`) that analyzed the data in the data set to see if a pattern existed between the features and the target variable. If not, we returned to step 1.
3. Using what we learned from the analysis script, we would have filled in the unknown numbers in our decision-tree pseudocode and written a script that takes as input a set of feature values and outputs a predicted target value. This would have been the "model" we would have sent to your sister.

In using our model, your sister probably would find that it didn't do a terrific job of predicting the selling price of a new home coming on the market. This is because the steps we took didn't involve looking for the *best* model we could have built; we simply built *a* model based on the first pattern we found. An experienced data scientist would iterate and ask, "Ok, my first model works, but is there a better selection of features (or even a superior statistical approach) that produces a better predictor?"

A good ML library automates much of steps 2 and 3 for you. We'll use the `pandas` and `scikit-learn` libraries, and with their help, this process turns into the following:

1. Review and analyze our data set using the `pandas` library, and pick one or more features we think might correlate well with the target variable.
2. Select the type of statistical analysis we want to use. We'll use the `DecisionTreeRegressor` class in `scikit-learn`. Besides decision trees, this library supports many other statistical techniques, and we'll select a different one when we later build a predictor for the toxicity of online comments.
3. Create an instance of the `DecisionTreeRegressor` class and call the resulting object's `fit` method, giving it the set of features and the target variable we want it to use from our training data.
4. Test this object, which is a model fit to our training data, against some previously unseen data, and evaluate whether this model is a good one. If it is not, we'll return to an earlier step, changing perhaps the features included or the type of statistical analysis performed. 
5. Share our best model with your sister. 

```{margin} Selecting the Best Features
Even the task of choosing the features (step 1) can be partially automated; if you take a ML course, you'll learn about statistical techniques that will automatically prioritize the features in your data set.
```

Notice that we no longer need to write the code that implements a statistical analysis (step 2) or the model that fits to the training data (step 3), as we tried to do earlier. Using powerful ML libraries like `pandas` and `scikit-learn`, our scripts need make only a few function calls.

## Getting a feel for the data

With this foundation, you are ready to start using the tools of a data scientist. We'll begin with [the pandas library, which "is a fast, powerful, flexible and easy to use open source data analysis and manipulation tool, built on top of the Python programming language."](https://pandas.pydata.org/) It provides data structures and functions that allow us to quickly explore a data set.

```{tip}
Many data scientists work in interactive Python notebooks (i.e., `ipynb` files) when doing ML, and we'll do the same in the rest of this chapter. They also commonly refer to the `pandas` library with the abbreviation `pd`.
```

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 2nd code block
import pandas as pd
```

```{margin} Terminology
Each column in a `DataFrame` is a `pandas.Series` data structure, and data scientists will often refer to a column as a series.
```

The most important data structure in the `pandas` library is the `DataFrame`. You can think of a `DataFrame` as resembling a table, like a worksheet in Excel notebook. The `pandas` library provides input/output functions, like `pandas.read_csv`, that allow you to pour data into and pull data out of a `DataFrame`. And once your data is in a `DataFrame`, you can apply many of the library's powerful functions, such as `pandas.DataFrame.describe` that creates a summary description of the data in each column. The following code block shows how we can use the `pandas` library to begin exploring the data set containing home sales in Ames, Iowa.

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 1st and 3rd code blocks
# Specify the input CSV file for building and testing the model
csv_file = 'AmesIowaHousingData/AmesHousing.csv'

# Read the CSV data into a DataFrame
df = pd.read_csv(csv_file)

# Print a summary of the data
df.describe()
```

``` {admonition} You Try It
Remember that the interactive Python interpreter will print the result of the last statement in a code block when you execute it in a `ipynb` file. Make sure you run the two previous code blocks, adjusting the pathname in the `csv_file` string to match where you placed the housing data set. You'll need to see the returned result for `df.describe()` to follow the explanation that comes next.
```

The results of our call to `df.describe()` list eight numbers for a large number of our data set's columns. The `pandas` library decided that each of these columns is a *numeric* series, and the numbers under the series name tell you some statistical facts about each of these columns. Here's a short summary of the meaning of each row in the table returned by `describe`:

* `count` reports the number of rows (in the indicated column from the data set) with *non-missing values*. As we discussed in Chapter 8, there are many reasons why a data set may contain missing values. In our data set, for example, two of the home sales don't report any value for `'Bsmt Full Bath'`, which might have occurred because the person filling out the data did't think that they needed to fill out this column in a house without a basement.
* `mean` is the arithmetic average of the non-missing values.
* `std` is the standard deviation of the non-missing values, and it provides a measure of the values' numerical spread.
* `min`, `25%`, `50%`, `75%` and `max` are best considered together. If you were to sort the non-missing values in the column from the smallest number to the largest, the first number in the sort is `min` and the last is `max`. Then imagine drawing three lines that split the sorted list into four equal-sized buckets. The number at these three split points are the values at the 25th percentile (25%), the 50th percentile (50%), and the 75th percentile (75%). Intuitively, the 25th percentile is the number that is bigger than a quarter of the column's values and smaller than the other three-quarters.

Knowing this, I can see that my copy of the Ames-Iowa-Housing data contains 2930 entries and the average home sale price between 2006 and 2010 was approximately 180,800 dollars, with the cheapest home selling for just under 80,000 dollars and the most expensive one for 755,000 dollars.

The `pandas` library limits the number of `DataFrame` columns and rows it displays, and if we wanted to see the statistics about bedrooms above ground (`'Bedroom AbvGr'`) that we analyzed with our own script earlier, we'd either have to change the maximum number of columns that `pandas` displays or slice that series from the result of the `describe` method call. Let's choose the latter approach:

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 5th code block
# Review only the statistics for number of bedrooms
df.describe()['Bedroom AbvGr']
```

From this result, we can see that 2- and 3-bedroom houses are common, and that we didn't need to worry about tracking houses with more than eight bedrooms.

## Set the prediction target

We're now ready to define the setup we'll use in configuring our prediction model. Let's start with the model's output, which is the variable we want to predict. The series labeled `'SalePrice'` is this target variable, and by convention, data scientists call it `y`.

To set what `y` names, we can grab that series from our `DataFrame` using one of two notations: (1) square brackets to slice it out, as we've done with other sequences; or (2) what's called *dot notation* in `pandas`. The former always works, while the latter is a nice shorthand when the series name doesn't contain spaces or conflict with another attribute name in the `pandas` library. The following code block illustrates both methods while using the latter since this label mets the requirements for using the shorthand.

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 6th code block
# Setting the prediction target
# y = df['SalePrice']
y = df.SalePrice
y
```

## Pick some features

As we did when we tried to solve this problem by writing our own code, we need to choose which data set features we think will correlate nicely with our target variable. Almost every series is a candidate except the first, which in my copy of the data set is called `'Order'`, and the last, which is the target variable. I eliminate the first series from contention since it is nothing more than each record's index in the data set. You'd want to eliminate anything like it when it's clear that that series could not possibly correlate with the target variable.

To choose among the others, let's interrogate the `columns` attribute on a `DataFrame`, which prints all the column labels.

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 7th code block
df.columns
```

As you'll find in many big data sets, there are a lot of columns to consider. While it might be tempting to include all the eligible columns in your model, it's not always the best choice. Let's begin with just a few as we can always later change the list of features included and compare the performance of our different models.

In the next code block, we build a list of column names that'll represent the features we want included in our model, and then slice those series out of our `DataFrame`. Again, by convention, these data are called `X`. This and the following code block invoke the `describe` and `head` methods on the `DataFrame` called `X` to check what we've done.

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 8th code block
# The model's input features
feature_names = ["Lot Area", "Year Built", "1st Flr SF", "2nd Flr SF", "Full Bath", "Bedroom AbvGr", "TotRms AbvGrd"]
X = df[feature_names]
X.describe()
```

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 9th code block
X.head()
```

```{tip}
As we've learned, frequent checks of our script's state help us to find the mistakes in our logic before we done too much work. These sorts of checks are even more important in the work of a data scientist, since it is harder to spot a model built from non-sensical data. Check the contents of your data frames frequently!
```

## Fit the model to our data

We chose to use a decision tree, which the `scikit-learn` library provides through the `DecisionTreeRegressor` class. Instantiating an instance of this class is the first step in creating a model that uses decision trees.

Hidden in this creation step, however, is a bit of randomness, which many ML libraries use as a best practice. Since we want to compare the models we build against each other, we want to eliminate this randomness as a potential cause of any difference in the performance of our models. We can accomplish this by specifying the random seed that the model should use each time it creates a new instance for us. This is the purpose of the `random_state` assignment in the constructor call in the following code block. You can choose any number you'd like as long as you consistently use it in all your constructor calls.

```{margin} Coding with `scikit-learn`
The Python module you want to import to use the `scikit-learn` library is called `sklearn`.
```

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 10th code block
from sklearn.tree import DecisionTreeRegressor

# Create an untrained model
my_model = DecisionTreeRegressor(random_state=42)

# Fit the model to the specified portion of the training data
my_model.fit(X, y)
```

Given a newly created instance of `DecisionTreeRegressor`, we simply pass our `X` and `y` variables to this object's `fit` method, and it updates the model. We can now use this object to make predictions!

## Predicting unseen data

To make a prediction using this fitted model, we need a set of values for the model's input features. Inputting these feature values will cause the model to return a prediction for the target variable. In other words, have someone tell us about a house we haven't yet seen, and for that house, give us its lot size, year it was built, square feet of living space in the first and second floors, and the number of full bathrooms, bedrooms above ground, and total rooms above ground. That's the list of features we used to train our model to predict selling prices.

Ok, but we used all the data we had about home sales in Ames, Iowa to create the model. We could go back to your sister for more data, but there's another way: data scientists take a given data set and split it in two. One part of this split is designated the *training set* and the other is the *test set*. We'll use the former to create our model and the latter to check its performance.

There are many ways to poorly split a data set, and to avoid these pitfalls, the train-test split routines in ML libraries like `scikit-learn` employ randomness. Again, we want to control the random seed used by these functions so that we can ensure that all the models we build are fed the same training and testing data sets.

The code block below retrains our model using the `train_X` and `train_y` data sets produced by the `train_test_split` function in the `scikit-learn` library. It then illustrates how to use the `predict` method on our model object, into which we feed the `text_X` data set. The resulting `predictions` are what we want to compare against the actual sale prices stored in `test_y`. I've written a little loop that does this comparison, converting a few data types along the way (i.e.., the `Series` in `test_y` into a Python list and each `float` in `predictions` into an `int`).

```{margin} Don't Test with Your Training Data
A model's utility is defined by its ability to make accurate predictions on data it hasn't previously seen, which are called the *validation data*. It's not hard to get good predictions from a model's training data. Try it by calling `predict` with `train_X.head()` instead of `test_X`.
```

```{code-block} python
---
lineno-start: 1
---
### chap17/ames.ipynb, 11th code block
from sklearn.model_selection import train_test_split

# Split both features and target data into training and validation sets
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
Run the code above. Are you happy with the resulting predictions?
```

## Model validation

To be written.

## Making the fit just right

To be written.

## Bias in ML

To be written.

\[Version 20240101\]