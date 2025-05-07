---
layout: post
title: Exploring the F1 Dataset
date: 2025-05-06 10:30:01
description: Initial work done using the Fast F1 API to explore F1 data
tags: F1, EDA, F1-Project, Development
categories: Research
---

# Fast F1

Sometimes you get a dataset (should it be data set?) that is just huge. And that seems to be the case with the [Fast F1](https://docs.fastf1.dev/index.html) dataset. This thing will give you lap times, car telemetry, weather data, tyre data, and not just for the race, for the practice session, and the qualification races too. It will even give you information on a team colour!

So yes, a large amount of data. I find that when I have a dataset that is huge, I need to play around with it, dig into it to see what I can find, to see how I can use it. And that is what I have been doing over the last few days, on-and-off.

The first thing I wanted to do was a bit of a comparison between the same race in different years. No real reason for picking this, but, it seemed like a good place to start,so I picked the race in Australia for 2025, 2024 and 2023. I picked this race because we are not that far into the 2025 season, and the Australian race this year was quite notable.

In 2024 and 2023, the Australian race was the third race in the calendar, but it was the first race in 2025.

Before we can start to analyse any data, we need to set that data. With the Fast F1 api, you need to get the session data, and then you need to load that data into a cache folder (which you create yourself). You can get the session data as follows:

```python
australia_2025 = fastf1.get_session(2025, 'Australia', 'R')
australia_2024 = fastf1.get_session(2024, 'Australia', 'R')
australia_2023 = fastf1.get_session(2023, 'Australia', 'R')
```

Here, you pass in the `year` as an `int`, the `race` as a `str`, however, it can also be an `int` if you know what race it was in the sequence of races. So, `3` for the 2023 and 2024 races, and `1` for the 2024 race. The final paramater I am passing in is `'R'`, for race. If you wanted qualifying data, you could pass in `'Q'`.

Once you are all set, you need to load the data, as follows:

```python
australia_2025.load()
australia_2024.load()
australia_2023.load()
```

And then you are set.

## The fastest Lap

The fastest lap is quite an obvious, and interesting place to start. With the Fast F1 api you can access the fastest lap time, sure, but you can also access the lap speeds, allowing you to plot the profiles.

To get the fastest lap, we can do the following:

```python
australia_2025_fLap = australia_2025.laps.pick_fastest()
australia_2025_fLap_dvr = australia_2025_fLap["Driver"]
australia_2025_fLap_time = australia_2025_fLap["LapTime"]
```

And basically repeat this for each year we are interested in. With a bit more work to get the speed data from the car, we can get the fastest laps for the last three races in Australia:

![Fastest Lap Plot](/assets/img/Blog/aust_fastest_lap.png)

From the graph:

- NOR: Lando Norris, McLaren driver
- LER: Charles Leclerc, Ferrari driver
- PER: Sergio Pérez, Red Bull driver

You will have to excuse the fact that the timings still have "0 days", I will figure out how to remove that later on.

There are a few interesting things about this plot:

1. I would have expected Max Verstappen to have had the fastest lap either in 2023 or 2024, given he was world champion in 2023 and 2024. But then, you will wear your tyres down pushing so hard, and maybe e did not need to do that.
2. Why is there a 2-3 second difference between the 2023 and 2024 races when compared to the 2025 race, and is this a significant amount of time?
3. Can we understand why Leclerc, in 2024, was hitting such high speeds in the mid-section? Was this typical of all drivers in that race?

Some of these questions will be examined here, some will be looked at later.

## What happened in 2025?

So why did Norris have the fastest lap in 2025, yet was about 2 seconds slower than the 2023 and 2024 races. One thing that can have a big impact on lap times is the track conditions -- if there is something that can impact a drivers ability to put the car where they want in the track, they will have to be a bit more careful. One thing the Fast F1 api lets us look at is the weather conditions, so we can see if it was raining in all three years of the race we are looking at. Once you have the session data loaded. you can get the weather data quite easily:

```python
weather_data_2025 = australia_2025.weather_data
australia_rain_2025 = weather_data_2025[weather_data_2025["Rainfall"] == True]

weather_data_2024 = australia_2024.weather_data
australia_rain_2024 = weather_data_2024[weather_data_2024["Rainfall"] == True]

weather_data_2023 = australia_2023.weather_data
australia_rain_2023 = weather_data_2023[weather_data_2023["Rainfall"] == True]
```

![Rainfall](/assets/img/Blog/rainfall_weather_data_2023_2024_2025.png)

The Australian GP is 58 laps, so we can see from this plot that in 2025, there was rain for each lap of the race. It is important to note, that this will not mean every part of the track experienced rain for every minute of the race, more that there was rain on some part of the track across the race. Meanwhile, for 2023 and 2024, there was no rain during the race.

Okay, so rain will have played a factor in a slower race, but with a time delta of about 2 seconds? That doesn't seem like an awful lot. And to people who experience normal, everyday traffic, it isn't. But, is it a lot in terms of F1?

To answer this, I think looking at qualifying will give us some insights. The purpose of qualifying is to go the fastest over a single lap, with the objective of being on pole position. Or, getting as good a place on the grid as you can.

So, a single lap, as fast as you can. Similar to the fastest lap during the race. To get this data, we need to gather the qualifying dataset:

```python
australia_2025_q = fastf1.get_session(2025, "Australia", 'Q')
australia_2024_q = fastf1.get_session(2024, "Australia", 'Q')
australia_2023_q = fastf1.get_session(2023, "Australia", 'Q')
```

And then load that data:

```python
australia_2025_q.load()
australia_2024_q.load()
australia_2023_q.load()
```

Here, I am interested in the top three qualifiers for the Australian GP in each year. For 2025, I did the following:

```python
# Filter for the top 3 finishers
australiaQ3_2025_top_3 = australia_2025_q.results[australia_2025_q.results["Position"] <= 3]

# Select the LastName and Q3 columns
australiaQ3_2025_top_3 = australiaQ3_2025_top_3[["LastName", "Q3"]]
australiaQ3_2025_top_3
```

I got the `Q3` results as this is the qualifying phase that determines the positions of the top 10 drivers. Qualification happens on the Saturday, the day before the race, so, different weather conditions.

After working with the data a bit, I was able to plot the results. Here is the top 3 qualifiers for 2025:

![Q3_2025](/assets/img/Blog/australia_q3_2025.png)

Here we can see that Norris (McLaren) was quickest, followed by Piastri (McLaren), then Verstappen (Red Bull). If we note the time on the y-axis, we can that there is less that 0.4 seconds between the three drivers.

Looking at 2024:

![Q3_2024](/assets/img/Blog/australia_q3_2024.png)

A similar situation in terms of the time between the three drivers (i.e. < 0.4 seconds), with Verstappen (Red Bull) being quickest, followed by Sainz (Ferrari), then Pérez (Red Bull).

And finally, 2023:

![Q3_2023](/assets/img/Blog/australia_q3_2023.png)

Here, we see again, Verstappen (Red Bull) is the quickest, followed by Russel (Mercedes) and then Hamilton (Mercedes). Again, the time between the top three is about 0.4 seconds.

And this makes a bit of sense for a number of reasons. One, the teams will prepare their car for the best qualification lap possible, with just the right amount of fuel, new tyres and so one. And, two, the regulations for the cars will have been quite stable during these races, with the last major regulation change coming between 2021 and 2022.

of course, qualifying and a fastest lap in the race are not strictly the same thing, some of the reasons why I have listed above, but it does server to show how consistent the drivers can be, and how close, in terms of time, they can be. So, I think it is fair to say that fastest lap in 2025 being about 2-3 seconds slower than previous years is significant, expecially when you consider these cars can hit speeds in the region of 300kph (~185mph).

# Where am I going with this?

My plan here is to see if I can build a machine learning model that can be used to predict the race winners. To do this, I will really need to dig into the data, and understand what I am looking at, determine which features are best used in a predictive model and so on.

I plan to do a series of blog posts detailing this work, and then, post predictions ahead of the the next race (whenever that may be -- this is a distraction project for me!).

I will also include the project in the Projects section of this website, and link to the code in GitHub, at the appropriate juncture.

So, yes. As Charles Leclerc would say "Let's go!"
