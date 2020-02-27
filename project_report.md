# Project Report

We aim to interpret and visualize users' behaviour on dating website [LibimSeTi](https://www.libimseti.cz/). [Dataset](http://www.occamslab.com/petricek/data/) provided by the company contains 250 MiBs of profiles with their ratings and is freely available.

We describe in detail how users give and receive ratings, what are assumptions to become popular, what are differences between males and females if it is possible to split the network into smaller communities. Further research concerns about user classification based on his or her features - gender, sexual orientation, rating weight, etc.

We write all the code in Python with the usage of graph libraries (e.g. NetworkX, GraphLab) and machine learning library Scikit-learn. We perform some of the graph embeddings in Gephi. And for large graph algorithms, we utilize Spark that might be helpful while processing all the data.

## Preprocessing

### Data Structure
The data consisted of a directed network with total 135 000 nodes and 17 million edges, with each node (user) having also a categorical variable gender (Female, Male, Unknown). Each rating were given in the scale of 1-10 and the dataset's total size was 245 MBs and 2 MBs respectively.

The data itself was quite clean and didn't have missing values. Only peculiarities were the "Unknown" value for gender categories and some of the ratings were for non-existent profiles.

## Analysis

First, we spent a lot of time preprocessing and formatting the data. While the original data wasn't large per say, to group the data into specific subsets we needed to add many features to a single dataframe. For this we used Pandas with pain-stakingly hand-crafted logic.

After getting this large dataframe of various features we subsetted it into groups based on gender and other features, which we have then visualized as pie charts and histograms. **and some small plottings for communities eg eigen somethings**.

The very large amount of edges made deeper graph analysis quite difficult as it was possible to run the algorithms with only a tiny fraction of the data. Thus any subset we used would probably not be a good representative of the dataset as a whole, and their results unreliable. Just visualizing the network itself was quite tedious work, and the fact that the network was directed probably made it even more slower than an undirected graph. We found out through empirical experiments that a network of 15k - 50k edges was the largest possible for visualization using eg Gephi.

### Overview

The users (nodes) themselves were quite neatly evenly distributed amongst into females and males with unknown having 1/10 portion.

**pie chart of nodes with gender**

Looking at the edges we can see that the men and females also have equal amount of edges going out from them.

**pie chart of edges with gender**

Going deeper we can then split the ratings into 3 groups, with the thresholds for the different classes as following: 10-6 for **positive** rating, 6-4 for **neutral** and 4-1 for **negative**. Their distribution falls with an overwhelming majority being positive, the other two sharing the rest with 1/4 portions. The mean of all ratings being **ratings mean**.

Yet these ratings do not fall evenly on the individual nodes, and some have disproportionately large amount of positive or negative reviews. The rating distributions for the different genders shows that even genders do not give out ratings in perfectly distributed manner. A noticeable anomaly is the high ratings given to some males in the data, here being the highest bar in the histogram:

**histograms of gender rating distributions**

Taking the means of the ratings of the users and plotting it as a histogram shows that the data indeed follows the Central Limit Theorem and that the distributions of means follow the normal distribution.

**histograms of gender rating mean distributions**

One can then start to wonder, what is the underlying cause for these kind of fluctuations in the data. Perhaps the users with great images of their faces gain a better rating? And maybe there has to be more than one great image? Or that they have written interesting descriptions of themselves or in some other way, are more interesting than the average user.

But however this we cannot infer from this data, alas we can only guess the true indicators for the given ratings. What we can then do however, is to see if there a correlation between popularity (lot of incoming edges) and the average given ratings. This would at least prove that being popular is indeed an indicator for being well-rated in a dating site.

Also other interesting questions that came to our minds were, if there could be a way to subset and visualize LGBT (lesbian, gay, bisexual, and transgender) users by which genders they have rated more.

### Degree distribution

Although the graph was high in edges, its distribution was highly varied. The majority of the users (nodes) had ratings (weighted edges) of only a few dozen. Yet a small portion of users (so-called super nodes) had at best 35 000 out-going edges. This dissimilarity in graph degrees was an interesting phenomenon and we then set out to explore if this would follow a power-law distribution, as it has been often observed in graphs.

**picture**

Using then a normal Maximum Likelihood Estimator (MLE) model to estimate the power-law degree distribution's parameters we received values: **constant=x, scale=y**. Plotting the fitted probability density function against the dataset's degree disribution histogram (in log-scale) we are able to see, that the degree distribution indeed follows a power-law distribution, albeit the parameter estimates were a bit off and a mixture model might be better suitable. (derp couldn't get that plot to work)

**picture of the degree distribution with PDF of fitted MLE power-law distribution model** 

### Lesbian and gay users

Looking at the data we found that there were a smallish subset of users which had rated over 50% more of the other gender than their own. The issue with this method was, that it seems to be quite normal for any user to rate anyone else, so that determining the user's sexuality becomes quite difficult.

But using a threshold of 50% we could subset a quite likely proportion of people that are at least somewhat ambiguous in their sexuality. Then splitting them into two categories, based on their true gender so Male and Female we denote them as "Gay" and "Lesbian" subsets respectively.

For these two groups we found... something.

**pictures**

### Community analysis

A complete community analysis of the users was quite impossible with insufficient resources to run the algorithms with the complete dataset. So we instead used some representative subsets of the data, to visualize some proportions of the network and how they are connected.

### Node Classification

TBA



