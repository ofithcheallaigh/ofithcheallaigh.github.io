---
layout: post
title: The Future is Federated
date: 2025-09-08 10:30:01
description: Introduction to Federated Learning
tags: Federated-Learning, PhD
categories: Research
---

One of the main areas of my research is federated learning (FL), and I thought it would be good to discuss that for a bit, because it is still a small (but growing!) area in machine learning (ML) and AI.

While it is part of my research, I will be quite general in these posts—if I do some interesting work, and get it published, I will talk about it here. But until then, it is all a big, massive super secret!

Like other areas related to my work, this will be part of an ongoing series of posts (are we meant to call them substacks?). So, let’s go.

# As the Title Says, The Future is Federated
Federated learning is a machine learning technique that allows the users to maintain data privacy. In it’s most common architecture, a FL network is made up of a central server, and a series of clients. I tend to tell people to not think of the server in terms of a cloud service, or something like that. It is a bit different.

In a typical, large ML application, there will be some system which will gather data, be that sensors, or people, or whatever. That data would typically be transferred to a cloud service, where a model would train on the data. There is a problem in this: you have the data being transmitted over a network. This means there are privacy concerns as well as the cost of transmission. Depending on the application area, both of these things can be big issues. For example, if you worked in a medical facility where scans and medical images were used for predicting diseases, there would be a lot of red tape in getting permission to send that data to a central location.

And this is where FL comes in. One of the big benefits of FL is that the data privacy is preserved. It does this by in effect inverting the tradition ML application structure. In a FL network, instead of the data being sent to a central location to train, the data stays where it was collected, and the model is sent to the data.



To put this a bit more formally, in a FL network:

- The server sends the current model out to the clients.

- The clients collect their data.

- The clients will train their new data using the most recent model it has.

- Once trained, the client will send the model updates back to the server.

- When the server receives all the most recent updates, it was aggregate them, and create a new model.

- This new model is sent out to all clients.

This process will continue until some stopping condition is met.

This is a somewhat simplified version of the FL process—I haven’t discussed the many challenges present in this work. I have to have something with which to entice you back!

# Small, but Growing

As I mentioned above, FL is a small subfield of AI. In 2024 to 2025, Scopus has 189,661 articles for the search:

> TITLE-ABS-KEY ( artificial AND intelligence ) AND PUBYEAR > 2023 AND PUBYEAR < 2026

And for the same time period with the following search:

> TITLE-ABS-KEY ( federated AND learning ) AND PUBYEAR > 2024 AND PUBYEAR < 2027

There are 13,839 articles.

Not really a rigorous analysis, but hints at the scale of things.

But for me, the most interesting this is the number of articles for

> TITLE-ABS-KEY ( federated AND learning ) AND PUBYEAR = 2023

was 6,775.

And for

> TITLE-ABS-KEY ( federated AND learning ) AND PUBYEAR = 2024

was 20,721.

So this shows the level of growth and interest there is in the area of federated learning.

# Flower

Anytime I talk about FL, I always talk about [Flower](https://flower.ai/). Flower have developed a federated AI framework. To quote their site, Flower is:

A unified approach to federated learning, analytics, and evaluation. Federate any workload, any ML framework, and any programming language.

But it is more than that …

The people at Flower have done something really important, they have built a really great community of people interested in FL! And if anyone reads this, and is in anyway interested in doing some FL work, even just playing around with a few things, this is the place to start. Everyone at Flower is super friendly, they have a wonder Slack channel, and run lots of community events. I was able to attend the Flower AI Summit 2025 held in March, in London, and it was a fantastic few days! And depending where it is, I really hope to attend the Flower AI Summit 2026.

At the minute, I am building my own FL network for my experiments, but I started out using Flower, I ran some early analysis work using their system, and it was fantastic! Any questions I had, I asked on the Slack channel, and always got help.

I will always big these guys up!! Do check them out.

# That’s All for Now

So, that is the first substack on FL. Ultimately, my research is FL related, so everything will tie back into this topic at some stage. That basically means I will be writing a lot more about FL.

Stay tuned.