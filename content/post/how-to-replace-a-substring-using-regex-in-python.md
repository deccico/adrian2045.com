+++
title = "How to replace a substring using regex in Python"
date = "2010-06-06"
tags = [
    "regular expressions",
    "programming",
    "python",
    "snippet",
]
featured = false
+++

The problem: You match a string with your regex, but you need to replace just a portion of it. How can we do this?
The trick is simple, put the text you want to replace within “()” which means “group” in regex language. If the regex 
works, you can replace just that portion by using Python match information, like in the following example:


```python	
#the first group contains the expression we want to replace
pat = "word1s(.*)sword2"
test = "word1 will never be a word2"
repl = "replace"
 
import re
m = re.search(pat,test)
 
if m and m.groups() > 0:
  line = test[0:m.start(1)] + repl + test[m.end(1):len(test)]
  print line
else:
  print "the pattern didn't capture any text"
```

This will print: '***word1 will never be a word2***'

The group to be replaced can be located in any position of the string.