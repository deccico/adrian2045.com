+++
title = "GPT 3 Impressions"
date = "2021-01-08"
description = "GTP-3, an AI model from OpenAI delivered impressive results in natural language tasks. We discuss in this article how close it is from human-level intelligence. What are the risks of this system, its limitations and whether we are getting too close to live into the Matrix… 🙂"
tags = [
    "AI",
    "ml"
]
aliases = [
    "/gpt-3-impressions",
]    
thumbnail = "images/gpt3/giant_brains.png"
featured = true
+++

GTP-3, an AI model from OpenAI delivered impressive results in natural language tasks. We discuss in this article how close it is from human-level intelligence. What are the risks of this system, its limitations and whether we are getting too close to live into the Matrix… 🙂"
<!--more-->

![Giant Brains](/images/gpt3/giant_brains.png "Giant Brains")[^6]

[^6]: Image Source: http://timoelliott.com/blog/WindowsLiveWriter/MachinesThatThinkin1949_142F6/giant-brains,-or-machines-that-think_2.jpg

# Introduction

GPT-3[^3] is a powerful Natural Language Processing (NLP) system capable of reaching human-like level in many tasks such as text generation, translation, questions answering, arithmetic, etc.

[^3]: Language Models are Few-Shot Learners – https://arxiv.org/abs/2005.14165

This is a deep-learning model. Deep learning is an AI function that aims to imitate how the human brain works by creating a complex multi-level matrix of values. This means that its “reasoning” cannot be really understood. So whatever wisdom GPT-3 gained is lost in translation for our human brains.

The main goal of the Open-AI researchers was to augment its capabilities by scaling it beyond anything done so far. Even when it was not trained for any particular task the authors tested it for many “general intelligence capabilities”. While not perfect, using 175 billion parameters, GPT-3 did deliver impressive results.

# Versus Humans

When compared to humans, GPT-3 has consumed more information than anyone can in their whole lifetime. Its training dataset reached 45 terabytes. This included: web page data, books, Wikipedia and even Reddit links with three or more up-votes. There is an overlap on the sources but the intention was to give them a different weight to increase the overall model quality.

While GPT-3 has an advantage on its training size, it only shows modest results for it. Just imagine combining all that information with the learning efficiency of a human brain. On the other hand, AI models will only get better with time, so watch out team carbon…

# GPT-3 Approach and take away

The main take away of GTP-3 is that adding more information into a NLP model does increase its accuracy. In the image below we can observe how the accuracy improve with the model size.

![GTP-3 accuracy vs model size](/images/gpt3/gtp-3_accuracy.png "GTP-3 accuracy vs model size")

Another finding was that one or few task demonstrations in natural human language, called “one shot” or “few shots” usually ramps up the machine accuracy. We can easily link this characteristic to the way humans learn or solve different tasks. Just observing one or few demonstrations is usually enough for us to set our “model” in the right direction.

# On accessibility

GPT-3 is definitely impressive, and the dream of any engineer to play with or scammers to get hold of. Given its capabilities, I think it is wise that is not freely accessible. It’s licensed to Microsoft, but you can request access to its API. Not an easy task though.

Their authors point out potential misuse such as spam, fake-news generation, phishing, fraudulent academic writing and more. Just imagine ‘Nigerian princes’ scams at an automatic industrial level. Others [^1] point out the ability of governments to use these tools for automatic propaganda generation.

[^1]: All the News that’s Fit to Fabricate: AI-Generated Text as a Tool of Media Misinformation https://papers.ssrn.com

![Human accuracy detecting GPT-3 generated news](/images/gpt3/gpt-3-humans_ability_to_detect_model_generated_news_articles.png "GTP-3 accuracy vs model size")

In this graph from the GPT-3 paper, different GTP-3 based models are used to generate news articles. They differ from the amount of training data they had available. From GTP-3 Small to the full-blown GTP-3 with 175 billion parameters. With a 52% accuracy for the biggest model, we can see that humans are not capable to detect synthetic test. Other papers have reached the same conclusion [^1], [^2].

[^2]: Defending Against Neural Fake News https://arxiv.org/pdf/1905.12616.pdf

# Grover Propaganda Machine

![Grover propaganda machine](/images/gpt3/grover.png "Grover propaganda machine")

In the case of Grover [^2], another type of NLP bot, the authors show that their machine propaganda generator is even better than what humans can achieve.

# How far from general intelligence?

![Are we in 2045 yet?](/images/gpt3/gpt-3_general_intelligence.png  "Are we in 2045 yet?")

Singularity [^4] will come likely years after machines achieve human-level intelligence. Thus creating a super-intelligence reaction that would leave humans well behind unless we find a way to make those science-fiction cybernetic implants work.

[^4]: Singularity – https://en.wikipedia.org/wiki/Technological_singularity

So, are we close to human parity yet? According to its paper, GTP-3 has performed phenomenally in some cases and rather poorly in others. This was done without training for any particular tasks. As opposed to other Chess or Go playing bots. The clues that is given (one shot or few shots) are human like, so we can “talk” to the machine and this action indeed improve its understanding.

The question is how fast we can improve GTP-3 or other AI models. The paper mentions some promising ideas, such as different training techniques, or even fine-tuning. Likely, in few years time, more and more professions will become obsolete challenging not the whole human race but more and more at a time. Time to pray to our future AI overlords [^5]?

[^5]: Church – Way Of The Future: https://web.archive.org/web/20171228094408/https://www.wayofthefuture.church/
