+++
title = "The Egg of The Universe"
description = "I accidentally re-discovered the cosmic Egg Theory."
date = "2023-01-27"
tags = [
    "philosophy",
    "ai",
    "python",
]
featured = false
+++

I accidentally re-discovered the cosmic Egg Theory.
<!--more-->


Playing with GPT API I generated the following conversation:

```
/usr/local/bin/python3.11 /Users/adrian/src/gptplay/test.py 
AI: hi, how can I help you?
Me: I found an egg

AI: Congratulations! What kind of egg is it?
Me: A toy egg

AI: Ah, that's fun. What is inside the egg?
Me: The whole universe fits in the egg, yet the egg is within the universe. Can you explain that?

AI: This is a Zen and Buddhist concept called the Cosmic Egg. It symbolizes the unity between everything 
in the universe - that all things are connected and part of the same whole. 
It suggests that the universe is both within and beyond each individual.
```

Context, my three years old just said "I found an egg!" So I repeated it to GPT with a personal formulation for a 
philosophical theory (I was just having fun) that was already invented long ago... I searched for references but 
could not find the exact theory, except for a book with low reviews and a Wikipedia article with many references 
from many cultures.

This is the code I used:

```python
import os
import openai

openai.api_key = os.getenv("OPENAI_API_KEY")

start_sequence = "\nAI:"
restart_sequence = "\nHuman: "

conversation = "hi, how can I help you?" + os.linesep
print(conversation)

while True:
    conversation = conversation + input().strip() + os.linesep
    response = openai.Completion.create(
        model="text-davinci-003",
        prompt=conversation,
        temperature=0.9,
        max_tokens=150,
        top_p=1,
        frequency_penalty=0,
        presence_penalty=0.6,
        stop=[" Human:", " AI:"]
    )
    text_response = response.choices[0].text
    conversation = conversation + text_response + os.linesep
    print(text_response)
```


Related links I found: 
* https://en.wikipedia.org/wiki/World_egg
* https://www.amazon.com/Cosmic-Egg-complete-universe-everything/dp/B0B15BX47V
