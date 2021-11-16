# Sentiment Analysis for textual data

Data analysis often starts with **structured data **that’s** **already stored as numbers, dates, categories etc. However, unstructured data can yield crucial insights if you use appropriate techniques. Often you need to create a report for your CS team using NLP... In this tutorial, we'll run sentiment analysis on a textual dataset to figure out how positive and negative each phrase is, and turn the results into an interactive Datapane report.&#x20;

## The dataset

Let’s imagine we’re a data scientist working for a news company and we’re trying to figure out how ‘positive’ our news headlines are in comparison to the industry.

We’ll start with the [UCI News Aggregator dataset](https://www.kaggle.com/uciml/news-aggregator-dataset) which is a collection of news headlines from different publications in 2014. This is a fun dataset because it has articles from a wide range of publishers and contains useful metadata.&#x20;

```python
import pandas as pd

raw_data = pd.read_csv("~/uci-news-aggregator.csv")

# Convert UNIX timestamps in milliseconds since 1970 into datetimes
raw_data["TIMESTAMP"] = pd.to_datetime(raw_data["TIMESTAMP"], unit = 'ms')

# Add more informative category names
di = {"b": "business",
      "t" : "science and technology",
      "e" : "entertainment",
      "m" : "health"}
raw_data.replace({"CATEGORY": di}, inplace=True)

raw_data.info()
raw_data.head()
```

![](https://cdn-images-1.medium.com/max/1600/1\*6xK9VG0egNnCx\_WERYyGdw.png)

We have 8 columns and about 400k rows. We’ll use the ‘Title’ for the actual sentiment analysis, and group the results by ‘Publisher’, ‘Category’ and ‘Timestamp’.&#x20;

### **Classifying the headlines**

Through the magic of open-source, we can use someone else’s hard-earned knowledge in our analysis — in this case a pretrained model called the Vader Sentiment Intensity Analyser from the popular [NLTK](https://www.nltk.org/index.html) library.

To build the model, the authors gathered a list of common words and then asked a panel of human testers to rate each one on **valence **i.e. positive or negative, and **intensity **i.e. how strong the sentiment is. As the [original paper](http://comp.social.gatech.edu/papers/icwsm14.vader.hutto.pdf) says: :&#x20;

> \[After stripping out irrelevant words] this left us with just over 7,500 lexical features with validated valence scores that indicated both the sentiment polarity (positive/negative), and the sentiment intensity on a scale from –4 to +4. For example, the word “okay” has a positive valence of 0.9, “good” is 1.9, and “great” is 3.1, whereas “horrible” is –2.5, the frowning emoticon “:(” is –2.2, and “sucks” and “sux” are both –1.5.

To classify a piece of text, the model calculates the valence score for each word, applies some grammatical rules e.g. distinguishing between ‘great’ and ‘not great’, and then sums up the result.

Interestingly, this simple lexicon-based approach has equal or better accuracy compared to machine-learning approaches, and is much faster. Let’s see how it works!

```python
import nltk
nltk.download('vader_lexicon')
from nltk.sentiment.vader import SentimentIntensityAnalyzer as SIA

sia = SIA()

results = [sia.polarity_scores(line) for line in raw_data.TITLE]
scores_df = pd.DataFrame.from_records(results)
df = scores_df.join(raw_data, rsuffix="_right")

df.head()
```

In this code we import the library, classify each title in our dataset then append the results to our original dataframe. We have added 4 new columns:&#x20;

* **pos**: positive score component
* **neu**: neutral score component
* **neg**: negative score component
* **compound**: the sum of the three score components

As a sanity check, let’s take a look at the most positive, neutral and negative headline in the text by using pandas `idxmax` :&#x20;

```python
negative = df.iloc[df.neg.idxmax()]
neutral = df.iloc[df.neu.idxmax()]
positive = df.iloc[df.pos.idxmax()]
print(f'Most negative: {negative.TITLE} ({negative.PUBLISHER})')
print(f'Most neutral: {neutral.TITLE} ({neutral.PUBLISHER})')
print(f'Most positive: {positive.TITLE} ({positive.PUBLISHER})')
```

Running that code gives us the following result:&#x20;

```
Most negative: I hate cancer (Las Vegas Review-Journal \(blog\))
Most neutral: Fed's Charles Plosser sees high bar for change in pace of tapering (Livemint)
Most positive: THANK HEAVENS (Daily Beast)
```

Fair enough — ‘THANKS HEAVENS’ is a lot more positive than ‘I hate cancer’!

## Visualizing the results

When we're building our report, we need great visuals...

What does the distribution of our scores look like? Let’s visualize this in a couple of ways using the interactive plotting library [Altair](https://altair-viz.github.io/index.html):&#x20;

```python
import altair as alt

df["compound_trunc"] = df.compound.round(1) # Truncate compound scores into 0.1 buckets 

res = (df.groupby(["compound_trunc","CATEGORY"])["ID"]
        .count()
        .reset_index()
        .rename(columns={"ID": "count"})
      )

hist = alt.Chart(res).mark_bar(width=15).encode(
    alt.X("compound_trunc:Q", axis=alt.Axis(title="")),
    y=alt.Y('count:Q', axis=alt.Axis(title="")),
    color=alt.Color('compound_trunc:Q', scale=alt.Scale(scheme='redyellowgreen')), 
    tooltip=['compound_trunc', 'count']
)

stacked_bar = alt.Chart(res).mark_bar().encode(
    x = "CATEGORY",
    y=alt.Y('count:Q', stack='normalize', axis=alt.Axis(title="", labels=False)),
    color=alt.Color('compound_trunc', scale=alt.Scale(scheme='redyellowgreen')), 
    tooltip=['compound_trunc', 'CATEGORY', 'count'],
    order=alt.Order(
      # Sort the segments of the bars by this field
      'compound_trunc',
      sort='ascending')
).properties(width = 150)

hist
```

Here we’re showing both a histogram for the overall distribution, as well as a 100% stacked bar chart grouped by category. Running that code, we get the following result:&#x20;

![](https://cdn-images-1.medium.com/max/1600/1\*cM8fnztmpLYn4DHGMGtluA.png)

Seems like most headlines are neutral, and health has overall more negative articles than the other categories.&#x20;

To give more insight into how our model is classifying the articles, we can create two more plots, one showing a sample of how the model classifies particular headlines, and another showing the average sentiment score for our largest publishers over time:&#x20;

```python
# Plot a random sample of 5k articles
scatter = alt.Chart(df.sample(n=5000, random_state=1)).mark_point().encode(
    alt.X("TIMESTAMP", axis=alt.Axis(title="")),
    y=alt.Y('compound', axis=alt.Axis(title="")),
    color=alt.Color('compound:Q', scale=alt.Scale(scheme='redyellowgreen')), 
    tooltip=['TITLE', 'PUBLISHER','compound:Q', 'TIMESTAMP']
)

# Get the 10 largest publishers
largest_10 = (df.groupby(by=["PUBLISHER"])["ID"]
        .count()
        .reset_index()
        .rename(columns={"ID": "count"})
        .nlargest(10, 'count')
      )

# Truncate by 30-day periods
df["date"] = df['TIMESTAMP'].dt.floor(freq='30D')

line = alt.Chart(df[df.PUBLISHER.isin(largest_10.PUBLISHER)]).mark_line(clip=True).encode(
    alt.X("date", axis=alt.Axis(title="")),
    y=alt.Y('average(compound)', axis=alt.Axis(title=""), scale=alt.Scale(domain=(-0.15, 0.15))),
    color=alt.Color('PUBLISHER:O'), 
    tooltip=['PUBLISHER','average(compound):Q', 'date']
)

line
```

This is where Altair really shines - its declarative syntax means you can change just one or two keywords to get an entirely different view on the data. Running that code gives us the following result:&#x20;

{% embed url="https://datapane.com/u/johnmicahreid/reports/0keR9XA/scatter-and-line-plots/embed" %}

By creating interactive visualizations, you enable viewers to explore the data directly. They’ll be much more likely to trust your overall conclusions if they can drill down to the original datapoints.&#x20;

Looking at the publishers chart it seems that HuffPost is consistently more negative and RTT more positive. Hmmm, seems like they have different editorial policies…

## Creating a Datapane report

The final step is to package the results into an interactive Datapane report so that others can interact with and understand the data.&#x20;

After logging into our Datapane account, we'll wrap our plots inside `dp.Plot` blocks, add some additional pages and written context:&#x20;

```python
import datapane as dp

# dp.login(token="yourtoken")

dp.Report(
    dp.Page(
        dp.Text(
"""
# Sentiment Analysis of News Headlines
This report uses a sentiment analysis model to determine the positivity/negativity of news headlines from the [UCI News Dataset](https://www.kaggle.com/uciml/news-aggregator-dataset).         
"""
      ),
        dp.Group(
            dp.Plot(hist), 
            dp.Plot(stacked_bar),
            columns=2
        ),
        dp.Text("""
Scores are unimodal, with over 50% of headlines classified as 'neutral'. Health appears to be the most negative news category. 

## Examples and publishers

To explore individual headlines, hover over the individual scatter points below: 
        """
               ),
        dp.Plot(scatter),
        dp.Plot(line, label = "Top 10 publishers average monthly sentiment"),
        dp.Text("""
Of our top 10 publishers, it looks like Huffpost is most consistently negative, and RTT Today most positive.


## Next Steps
....
        
        """),
        title = "Charts"
    ), dp.Page(
        dp.DataTable(df),
        title = "Selected Data"
    )
).upload(name="Distribution_of_Sentiment")
```

Running that code gives us the following:&#x20;

{% embed url="https://datapane.com/u/johnmicahreid/reports/Okp4JXk/distribution-of-sentiment/embed" %}
