+++
title = "Multiline regex pattern"
date = "2010-05-06"
tags = [
    "regular expressions",
    "c#",
    "programming",
    "python",
    "snippet",
]
thumbnail = "images/posts/regex.png"
featured = false
+++

![](/images/posts/regex.png "")
 [^1]

[^1]: Source: https://xkcd.com/


Task: Parse a file and capture whatever text appears between a pair of double quotes like the following:

**“Catch me”**

Not so difficult, you could use the following regex:

```
“.*”
```

This will match any character within double quotes in a group <br />
¿any?

Well, if you have to deal with multi-line characters (CR / LF) like in the following text:

```
“Catch me
if you can”
```

The **.** special character means “any character” In fact means “any character, except for new lines”. 
So it won’t work in our case. And you can’t also put it within a set of characters like: **[.s]** which would mean:

```
. match any character
s plus any space character (including new lines)
```

The problem is that inside **[]** special characters like . or * or ? lose their meaning and are treated as 
literals.
Python address this issue with the option **re.DOTALL**, which makes a dot mean really any character even a new lines.
If you are working with other language with a regex library but without this option, like C# for instance you could 
use this trick:
**“[wW]*”**

The **[wW]** means: “match any word character + non word characters”. You could solve it by combining other special 
characters, but I find this way specially clear since there is no doubt that you will truly match any character as you 
are just adding two complementary sets.

# Making things more complex

If you have a file like this:

```
“Catch me
if you can”
“other line”
“and the last one”
```

The regex will match from the first double quote, to the last one. To solve it you shoud use a non greedy multiplier 
like: ***?**


When you use __*__ you are actually saying “match zero or more characters that fulfill the preceding condition.” 
But the regex engine will choose the longest match possible. This is because you are using a greedy quantifier. 
To solve this you should use a non-greedy quantifier, like in this regex:\
**“[wW]*?”** </br>
And to put anything (except the double-quotes) in a group (so you could for instance iterate over the results), just 
add some brackets after and before the quotes: 

**“([wW]*?)”**


# Code examples

In C# (Visual C# 2010) we need to do the following:

```c#	
using System;
using System.Text.RegularExpressions;</code>
 
namespace ConsoleApplication1
{
    class TestRegularExpressions
    {
        static void Main()
        {
            // double "" are used to escape double-quotes
            // "?" is used to give the capture text a simple name
            // @ means the text is a string literal and we don't want that C# escapes any character (like is usual when you write regex patterns)
            string pattern = @"""(?[wW]*?)""";</code>
 
            Regex regex = new Regex(pattern);
 
            string text = new System.IO.StreamReader(@"c:Usersadriantest.txt").ReadToEnd();
            /*
            * Suppose that c:\Users\adrian\test.txt has the following content:
            *
            "Catch me
            if you can"
            "other line"
            "and the last one"
            */
 
            Match m = regex.Match(text);
 
            //iterate in all the captures
            while (m.Success)
            {
                Console.WriteLine("Captured line: " + m.Groups["quoted_line"]);
                m = m.NextMatch();
            }
 
            Console.WriteLine();
 
        }
    }
}
```	

This will print:\
Captured line: Catch me\
if you can\
Captured line: other line\
Captured line: and the last one 

---

Of course in Python you have to do less effort to get the same result.

```python	
import re
 
pattern = r'"(?P.*?)"'
 
text = """"Catch me
if you can"
"other line"
"and the last one" """
 
# Retrieve group(s) by name
for m in re.finditer(pattern, text, re.DOTALL):
    print "Captured line: %s " % m.group("quoted_line")
```

---

The output is the same as before:

Captured line: Catch me\
if you can\
Captured line: other line\
Captured line: and the last one

Please note the differences between Python and C#:

* As we previously mentioned, you can use re.DOTALL to capture also new lines.
* To name a group “quoted line” you write in Python ?P<quoted_line> instead of the C# version ?<quoted_line>
* You write less and get more!

