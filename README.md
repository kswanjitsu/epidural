# Epidural Healthcare Sentiment

## Scraping: 
We scraped around 717,000 tweets with four different queries: "epidural," "medication-free birth," "natural birth," and "unmedicated birth." (see the notebook: scrapey.ipynb)

## Sentiment analysis: 
Afterward, we perform sentiment analysis on all of those tweets with the model: "cardiffnlp/twitter-roberta-base-sentiment-latest" (notes: it is important to pre-process the data the same way the model was trained, you can see the notebook "sentiment.ipynb" to see how this was done, or view the huggingface repo for the model; additionally I filter the datasets with LDA topic modeling after sentiment analysis -- this probably could have been done prior to sentiment analysis, but again because google collab was not sufficient for topic modeling, I figured I'd run the sentiment analysis first even if we throw away some of those tweets).

## Topic modeling: 
This is a common NLP method to look at topics within a corpus of documents (for us our documents are tweets). Various topic modeling approaches exist, but we went with LDA (attempted BERTopic but the results were nonsensical). Topic modeling is helpful for filtering because when scraping twitter you can get a lot of unneeded results (for example a lot of tweets were in spanish, but included the term epidural, it would be difficult to filter out all Spanish tweets by single keyword lexical matches). LDA models typically do require heavy pre-processing (i.e. filtering out stop words, lowercasing, getting rid of URLs, retweets, etc. I elected to not lemmatize for reasons that are probs beyond the scope of this email). I generated multiple models and selected the best model based on coherence score (0.502, 0 to 1, 5.0 is considered good) and perplexity score (-7.800 lower is better) 

You can view the results of the LDA model [here](https://epiduralbirthtwitterproject.tiiny.site/). From these results, you can see that topic 1 is the tweet dataset that we are interested in. I apply a filter to the total tweets dataset to only include tweets from topic 1. Finally, also within this script, to get rid of generic epidural tweets I manually remove any tweets that have the lexical matched keywords of "surgery," "stimulator," "steroid," "block." After LDA and manual filtering we are left with 431,524 tweets that pertain to topics involving epidural, labor, birth, natural/unmedicated birth, each with sentiment scores. The file for this dataset is in the repo and is titled: filtered_df.csv

## Semantic Search
I took the top 1000 top k matches for both "epidural birth labor," and "natural birth labor" as queries, which should be plenty to show a statistical difference, and top k is a valid sampling strategy. This should satisfy any stat nerd critiques. 

Of note, on sanity check/manual review most of the results check out, of course on sentiment analysis, some that probably could've been positive were rated neutral, however, the semantic search worked really well.

The output is in the format of a triple pipe delimiter to be safe on splitting (in case anyone used a pipe in their tweet): <epidural or natural>|||tweet content|||sentiment score

## TO DO:
[ ] Further filter epidural group to exclude "no epidural" and "without epidural."

[ ] I would split the results and calculate a mean for the sentiment of each group, "epidural" group," and "natural" group. Then do an unpaired (Welch's t-test) to compare the difference (if any) of sentiment between natural and epidural labor tweets.




