---
layout: post
title: Literature Review Prep
date: 2025-05-29 10:30:01
description: Some data analysis on Scopus data to prepare for a lit. review
tags: Literature-Review, Data-Analysis, PhD
categories: Research
---

# Introduction

Any research project will typically need a literature review (LR). The point of the LR is to provide an overview and analysis of the work done to date in that research area. Sometimes the LR can be extensive.

Since I have to carry out an extensive review, I through the best thing to do would be to get some data and do a bit of work so I could understand what journals are publishing the most articles in my research area, who are the main people working in my area, and so on.

The data was quite easy to get from Scopus<sup>[1](#myfootnote1)</sup>, once I figured out my search term (which was: `(TITLE-ABS-KEY("federated learning") AND (LIMIT-TO(PUBYEAR,2024))  AND (LIMIT-TO(DOCTYPE,"ar"))` for those interested). This search is for articles only. I am interested specifically in journal articles, mainly because it is best to limit your literature review to journals, since they are the highest standard of puiblication. This search returned 5743 articles, 5735 of which come from journals (7 are from trade journals, and 1 is from a book series). This analysis will only be considering the journal articles -- also, I have just downloaded the top 1000 returned results, and ranked by citation. I ranked by citation initially because I am looking for the most popular articles, and their sources.

Note: I found out after my initial work, I could filter futher for journal articles only with this search: `TITLE-ABS-KEY ("federated learning")  AND (LIMIT-TO (DOCTYPE, "ar")) AND (LIMIT-TO(SRCTYPE, "j"))`

## The Development of Federated Learning

One thing that stands out from just looking at the information Scopus returns in the growing interst in federated learning. This can be seen in the following graph, from Scopus:

![Journals by Year](/assets/img/Blog/scopus_fl_journals_by_year.png)

<div class="caption text-center">
    <i>Federated Learning Journal Articles Published by Year (Scopus)</i>
</div>  <br>
For 2024, Scopus listed 5735 joural articles in their database, as of May, 2025, they already have 6020 articles listed. So there is growing interest in federated learning (FL).

<br>

# A Look At the Data

## What Journals?

Scopus allows you to populate a page with 200 search results, so to get my data, I selected this, and exported the data to a `.csv` file. I did this for the first five pages, to get my 1000 results. Using `pandas` in python, I read in the `.csv` files with the `read_csv` function, and concatanted them into a single DataFrame.

Scopus also allowed me to export what I callthe meta data for my search results, in the form of a `.csv` file also, but I had a lot of issues reading that in with `pandas` -- there was some encoding issue it wasn't happy with, and the quickest way I could solve that was to save that data as an `.xlsx` files, and read it is using the `read_excel` function.

![Quick look at the data](/assets/img/Blog/scopus_fl_journals_dataset_head.png)

<div class="caption text-center">
    <i>Federated Learning Journal Dataset</i>
</div>  <br>

The reason for this work is to try and find out what journals are publishing the popular papers, what journals are publishing the most papers, who are the big names in the field.

So, the first thing I looked at was the paper from 2024 with the most citations. And this was:

```
Title: Heterogeneous Federated Learning: State-of-the-art and Research Challenges
Authors: Ye, Mang; Fang, Xiuwen; Du, Bo; Yuen, Pong C.; Tao, Dacheng
```

This paper was published in ACM Computing Surveys, and has 213 citations. This journal is quite interesting -- as the name suggests, they only publish surveys and the like. Their website says:

> Computing Surveys publishes comprehensive, readable tutorials and survey papers that give guided tours through the literature and explain topics to those who seek to learn the basics of areas outside their specialties.<sup>[2](#myfootnote2)</sup>

And:

> Computing Surveys does not publish "new" research.<sup>[2](#myfootnote2)</sup>

And surverys are great! They help you get an initial understanding, and most people starting off on a topic will use serveys. ACM state this! But surveys are not what I will want for my reviews. I will want original, new research. (Of course, surveys are helpful in guiding you tothat too!)

From here, I wanted to see what journals are publishing new research. These then to be the transactions in specific areas. I decided to look at the amount of papers that each journal has published, thinking this seemed like a good metric -- people want their work publiched in the best places, and will target them.

So:
```python
# Count the occurrences of each "Source title"
source_title_counts_1000 = paper_data_df["Source title"].value_counts()

# Display the counts
source_title_counts_1000.head(10)
```

The Top 10 are:

| Journal Title                                               | Paper Count  | Impact Factor  |
| ----------------------------------------------------------- | ------------ | -------------- |
| IEEE Internet of Things Journal                             | 98           | 8.2            |
| IEEE Transactions on Mobile Computing                       | 55           | 7.7            |
| IEEE Transactions on Consumer Electronics                   | 51           | 4.3            |
| IEEE Access                                                 | 33           | 3.4            |
| IEEE Transactions on Wireless Communications                | 30           | 8.9            |
| IEEE Transactions on Neural Networks and Learning Systems   | 25           | 10.2           |
| Future Generation Computer Systems                          | 23           | 6.2            |
| Expert Systems with Applications                            | 21           | 7.5            |
| IEEE Transactions on Information Forensics and Security     | 20           | 6.3            |
| ACM Computing Surveys                                       | 18           | 23.8           |

The top journal is `IEEE Internet of Things Journal`. Journals will public metrics to try and help researchers (and others) get a feel for how important the journal is, but it really isn't the only consideration when assessing how good a journal is. Other things, such as topic area and so on will come into play. But I think it is fair to say anything published by the IEEE or Elsevier, as all but the ACM Computing Surveys is, will be top quality papers. I know less about ACM, but given the impact factor, and the citations (which, I know, are linked), I am confident saying it is top quality too.

## Digging Deeper

So, not I am starting to get a feel for the types of journals I should be looking in. But, what else can the data tell us?

We can take a look at the subject areas where the federated learning articles are being published. The meta data is useful for the work going forward:

![Quick look at the data](/assets/img/Blog/scopus_fl_subject_areas.png)

<div class="caption text-center">
    <i>Number of FL Journal Publications by Subject Area</i>
</div>  <br>

I don't think it will really surprise anyone that Computer Science and Engineering are the top two. Mathematics surprised me a bit, but then, people working in applied mathematics are obviously doing some work on, I would assume, various algorithms and so on. Areas related to health make a lot of sense too, as they will benefit from the privacy preserving element of federated learning.

The next plot did surprise me though, and it is journal articles published by country:

![Quick look at the data](/assets/img/Blog/scopus_fl_journals_country.png)

<div class="caption text-center">
    <i>Number of FL Journal Publications by Country</i>
</div>  <br>

We can see that China is, by a long way, publishing more federated learning related journal articles than anyone else. To pie chart below drives this point home a but more:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="../assets/img/Blog/scopus_fl_journals_country_pie.png" class="img-fluid" %}
    </div>
</div>

<div class="caption text-center">
    <i>Pie Chart Showing the Number of FL Journal Publications by the Top 10 Country</i>
</div>  <br>

This chart looks at the Top 10 countries only, and we can see, of that 10, China is producing almost 50% of the work! America is only close to 13%, with India approaching 8% and the UK approaching 6%.

The code for the pie chart plot is:

```python
# Top 10 countries only
top_10_countries_df = scopus_meta_data_country_df.head(10)

# Plot a pie chart for the top 10 countries
plt.figure(figsize=(10, 6))
plt.pie(
    top_10_countries_df["Country Count"],  # values
    labels=top_10_countries_df["COUNTRY"],  # labels
    autopct='%1.1f%%',  # shows percentages
    startangle=90,  # rotate pie chart for better readability
    labeldistance=1.15 # distance the country names is from the edge of the chart
)

# Add title
plt.title("Top 10 Countries by Count", fontsize=14)

# Show the plot
plt.show()
```

From this, I was interested to see what universities were producing the most papers (code then chart):

```python
scopus_meta_data_affiliation_df
top_20_affiliations_df = scopus_meta_data_affiliation_df.head(20)

# Plot a bar chart for "Subject Area" vs "Subject Area Count"
# top_30_countries_df = scopus_meta_data_country_df.head(30)
plt.figure(figsize=(10, 6))
plt.bar(top_20_affiliations_df["AFFILIATION"], top_20_affiliations_df["Affiliation Count"], color='skyblue')

# Add labels and title
# plt.xlabel("Affiliation", fontsize=12)
plt.ylabel("Publication Count", fontsize=12)
plt.title("Number of Journal Publications by Affiliation", fontsize=14)
plt.xticks(rotation=45, ha='right')  # Rotate x-axis labels for better readability
plt.show()
# Show the plot
```

![Quick look at the data](/assets/img/Blog/scopus_fl_affiliations.png)

<div class="caption text-center">
    <i>Pie Chart Showing the Number of FL Journal Publications by the Top 10 Country</i>
</div>  <br>

We can see that the `Ministry of Education of the People's Republic of Chain` is producing about twice the output of the next university, the `University of Electronic Science and technolofy of China`.

## Keywords

The last bit of work for now, is looking at the keywords associated with these 1000 articles. The Word Cloud below shows these:

![Quick look at the data](/assets/img/Blog/scopus_fl_keywords.png)

<div class="caption text-center">
    <i>FL Journal Publication Keywords</i>
</div>  <br>

There are a few things that stuck out to me here, specifically the `Unmanned_Aerial_Vehicle_(UAV)` keyword, and others like `Sematics`, `Knowledge_Distillation` and `Adversarial_Machine_Learning`. These terms will be useful when I am searching for newly published work.

## Conclusions?

The point of this work was to understand what journals I should be looking at when it comes to gathering papers for my literature review, and I think I have that information. There is still some more analysis I want to do, mainly around the individual who are doing the work, their labs and so on.

From there, I can maybe set up alerts for work coming from these places, and from certain people.

I was very surprised at how much more work is coming out of China, relative to America. Of course, this analysis was done just looking at FL specificially, but I get the impression from this, and from the news, that China is moving ahead with real momentum.

Hopefully, more to come. Soon!

## Footnotes

<a name="myfootnote1">1</a>: From the Scopus webiste: Scopus is a "Comprehensive, multidisciplinary, trusted abstract and citation database". Researchers and other interested parties can use Scopus to look for peer-reviewed published work om a wide range of topics.

<a name="myfootnote2">2</a>: Source link: https://dl.acm.org/journal/csur/editorial-charter