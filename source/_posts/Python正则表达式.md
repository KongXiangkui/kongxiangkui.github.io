---
title: Python正则表达式
date: 2020-03-24 21:22:49
tags: 
- Python
- 正则表达式
categories:
- [Python, 语言]
toc: true
---
本文介绍正则表达式的相关知识，以及Python使用正则表达式进行编程（基于re库）
<!--more-->
## 不用正则表达式的模式匹配

```Python
# isPhoneNumber.py
def isPhoneNumber(text):
    if len(text) != 12:
        return False
    for i in range(0, 3):
        if not text[i].isdecimal():
            return False
    if text[3] != '-':
        return False
    for i in range(4, 7):
        if not text[i].isdecimal():
            return False
    if text[7] != '-':
        return False
    for i in range(8, 12):
        if not text[i].isdecimal():
            return False
    return True

if __name__ == '__main__':
    print('415-555-4242 is a phone number:')
    print(isPhoneNumber('415-555-4242'))
```

```Python
# test.py
message = 'Call me at 415-555-1011 tomorow. 415-555-9999 is my office.'
for i in range(len(message)):   # for i in range(len(message) - 12)
    chunk = message[i:i+12]
    if isPhoneNumber(chunk):
        print('Phone number found: ' + chunk)
print('Done')
# result:
# Phone number found: 415-555-1011
# Phone number found: 415-555-9999
```

## 采用正则表达式Regular Expressions，regexes

Regex object
- re.compile
- regexObj.search('string')
- regexObj.findall('string')

Character Classes:

\d stand for a digit character (0 ~ 9)
\D stand for any character that is not a numeric digit from 0 to 9
\w Any letter, numeric digit, or the underscore character.
\W Any character that is not a letter, numerica digit, or the underscote character.
\s Any space, tab, or newline character.
\S Any Character that is not a space, tab, newline

\d\d\d
\d{3} stand for "Match this pattern \d three times"
{ha}{3, 5} 匹配3到5个ha，即hahahha,hahahaha,hahahahaha


|(Pipe) 匹配多个，返回第一个

*(star) means "Match zero or more"，匹配0个及以上
+(plus) means "Match one or more，匹配一个及以上
?   表示这个group是可以有，可以无的

> Python’s regular expressions are greedy by default, which means that in ambiguous situations they will match the longest string possible.
> The nongreedy version of the curly brackets, which matches the shortest string possible, has the closing curly bracket followed by a question mark(?). 

自定义Character Class

- class [aeiouAEIOU] match any vowel, both lowercase and uppercase.
- calss [a-zA-Z0-9] match all lowercase letters, uppercase letters, and numbers.(使用hyphen-表示一个range)

^(caret) 表示为取反
- class [^aeioAEIOU] 表示除了元音字母之外的字符

> 在方括号内部，匹配. ? ( ) 不需要反义符

^(caret): 用在正则表达式的开头，表示必须在文本的开头进行匹配
$(dollar): 用在正则表达式的结尾，表示必须在文本的结尾进行匹配
同时使用，表示整个字符串都要匹配

.(dot) wildcard, any single character except the newline匹配任何一个字符，除了newline
.*(dotstar) everything and anything (greedy mode)
.*? match any and all text in a nongreedy fashion

re.compile('.*', re.DOTALL) 匹配一切包括换行符
re.compile('.*', re.IGNORECASE) 或者 re.compile('.*', re.I) 匹配时忽略大小写


several steps to using regular expressions in Python:

1. Import the regex module with import re.
2. Create a Regex object with the re.compile() function. (Remember to use a raw string.)
3. Pass the string you want to search into the Regex object’s search() method. This returns a Match object.
4. Call the Match object’s group() method to return a string of the actual matched text.


```Python
import re
# Regex object
phoneNumRegex = re.compile(r'\d\d\d-\d\d\d-\d\d\d\d')  # \ backslash 反义符 '\\d\\d\\d-\\d\\d\\d-\\d\\d\\d\\d'
mo = phoneNumRegex.search('My number is 415-555-1011')

# Grouping with Parentheses
print('Phone number found: ' + mo,group())
# Phone number found: 415-555-1011
print(mo.group(1))
# '415'
print(mo.group(2))
# '555-1011'
print(mo.group(0))
# '415-555-1011'
mo.groups()
# ('415', '555-1011')
areaCode, mainNumber = mo.groups()
# areaCode = , mainNumber 

phoneNumRegex.findall('Cell: 415-555-1011 Work:  415-555-1287')
# ['415-555-1011', '415-555-1287']
# 返回值为字符串列表

phoneNumGroupRegex = re.compile(r'(\d\d\d)-(\d\d\d)-(\d\d\d\d)')
phoneNumGroupRegex.findall('Cell: 415-555-1011 Work:  415-555-1287')
# [('415', '555', '1011'), ('415', '555', '1287')]
```

```Python
import re
phoneNumRegex = re.compile(r'(\d\d\d)-(\d\d\d-\d\d\d\d)')
mo = phoneNumRegex.search('My number is  415-555-1011.')

```

```Python
import re
heroRegex = re.compile(r'Batman|Tina Fey')
mo1 = heroRegex.search('Batman and Tina Fey.')  # 有两个都可以匹配，则返回第一个匹配值
mo1.group()
# 'Batman'

mo2 = heroRegex.search('Tina Fey and Batman.')  
mo2.group()
# 'Tina Fey'
```

```Python
import re
batRegex = re.compile(r'Bat(man|mobile|copter|bat)')
mo = batRegex.search('Batmobile lost a wheel')
mo.group()
# 'Batmobile'
mo.group(1)
# 'mobile'
```

```Python
import re
barRegex = re.compile(r'Bat(wo)?man')
mo1 = batRegex.search('The Adventures of Batman')
mo1.group()
# 'Barman'
mo2.batRegex.search('The Adventures of Batwoman')
mo2.group()
# 'Barwoman'
```

```Python
import re
batRegex = re.compile(r'Bat(wo)+man')
mo1 = batRegex.search('The Adventures of Batwoman')
mo1.group()
# 'Barwoman'
mo2.batRegex.search('The Adventures of Batwowowoman')
mo2.group()
# 'Barwowowoman'
mo3.batRegex.search('The Adventures of Batman')
mo3.group()
# None
```

```Python
import re
greedyHaRegex = re.compile(r'(Ha){3,5}')   
mo1 = greedyHaRegex.search('HaHaHaHaHa')
mo1.group()
# 'HaHaHaHaHa'
nongreedyHaRegex = re.compile(r'(Ha){3,5}?')
mo2 = nonHaRegex.search('HaHaHaHaHa')
mo.group()
# 'HaHaHa'
```

```Python
vowelRegex = re.compile(r'[aeiouAEIOU]')
vowelRegex.findall('RoboCop eats baby food. BABY FOOD')
# ['o','o','o','e','a','a','o','o','A','O',O']
noneVowelRegex = re.compile(r'[aeiouAEIOU]')
noneVowelRegex.finall('RoboCop eats baby food. BABY FOOD')
# ['R', 'b', 'C', 'p', ' ', 't', 's', ' ', 'b', 'b', 'y', ' ', 'f', 'd', '.', ' ', 'B', 'B', 'Y', ' ', 'F', 'D']
```
```Python
import re

# ^开始
beginWithHello = re.compile(r'^Hello')
beginWithHello.search('Hello world')
# <re.Match object; span=(0, 5), match='Hello'>
beginWithHello.search('He said hello world') == None
# True

# $结束
endWithNumber = re.compile(r'\d$')
endWithNumber.search('You number is 42')
# <re.Match object; span=(15, 16), match='2'>
endWithNumber.search('Your number is forty two') == None
# True

# ^ $ 共同作用
wholeStringIsNumber = re.compile(r'^\d+$')
wholeStringIsNumber.search('1234567890')
# <re.Match object; span=(0, 10), match='1234567890'>
wholeStringIsNumber.search('1234ert4567') == None
# True
wholeStringIsNumber.search('1234     12234') == None
# True
```

```Python
import re

atRegex = re.compile(r'.at')
atRegex.findall('The cat in the hat sat on the flat mat. \n at \natm.')
# ['cat', 'hat', 'sat', 'lat', 'mat', ' at']

nameRegex = re.compile(r'First Name: (.*) Last Name: (.*)')
mo = nameRegex.search('First Name: AL Last Name: Sweigart')
mo.group(1)
# 'AL'
mo.group(2)
# 'Sweigart'

nogreedyRegex = re.compile(r'<.*?>')
mo1 = nogreedyRegex.search('<To serve man> for dinner.>')
mo1.group()
# '<To serve man>
greedyRegex = re.compile(r'<.*>')
mo2 = greedyRegex.search('<To serve man> for dinner.>')
mo2.group()
# '<To serve man> for dinner.>'

# matching newlines with dot character
noNewlineRegex = re.compile('.*')
noNewlineRegex.search('Serve the public trust.\nProtect the innocent. \nUphold the law.').group() 
# 'Serve the public trust.'
newlineRegex = re.compile('.*', re.DOTALL)
newlineRegex.search('Serve the public trust.\nProtect the innocent. \nUphold the law.').group() 
# 'Serve the public trust.\nProtect the innocent. \nUphold the law.'

# re.I或者re.IGNORANCE 忽略大小写
robocopRegex = re.compile(r'robocop', re.I)
robocopRegex.search('RoboCop is part man, part machine, all cop.').group()
# 'RoboCop'
robocopRegex.search('ROBOCOP is part man, part machine, all cop.').group()
# 'ROBOCOP'
```

Substituting Strings with the sub() method
- 第一个参数：a string to replace any matches
- 第二个参数: the string for rhe regular expression
- 返回值: 替换之后的字符串

```Python
import re
nameRegex = re.compile(r'Agent \w+')
nameRegex.sub('CENSORED', 'Agent Alice gave the secret documents to Agent Bob.')
# 'CENSORED gave the secret documents to CENSORED.'

# in the first parameter, you can type \1, \2, \3, and so on, to mean "Enter the text of group 1,2,3,and so on, in the subsitution."
# \1表示第一个group
# (\w)\w 一个word和多个word
agentNameRegex = re.compile(r'Agent (\w)w*')
agentNameRegex.sub(r'\1****', 'Agent Alice told Agent Carol that Agent Eve knew Agent Bob was a double agent.')

# \2表示第二个group
agentNameRegexTest = re.compile(r'Agent (\w)(\w)\w*')  
agentNameRegexTest.sub(r'\2****', 'Agent Alice told Agent Carol that Agent Eve knew Agent Bob was a double agent.')
# 'l**** told a**** that v**** knew o**** was a double agent.'
```

复杂的正则表达式，可以通过忽略正则表达式中的 whitespace 和 comments (使用verbose mode)
re.VERBOSE

```Python
import re

phoneRegex = re.compile(r'((\d{3}|\(\d{3}\))?(\s|-|\.)?\d{3}(\s|-|\.)\d{4} (\s*(ext|x|ext.)\s*\d{2,5})?)')

# 分行，便于阅读
phoneRegex = re.compile(r'''(    
    (\d{3}|\(\d{3}\))?            # area code    
    (\s|-|\.)?                    # separator    
    \d{3}                         # first 3 digits    
    (\s|-|\.)                     # separator    
    \d{4}                         # last 4 digits    
    (\s*(ext|x|ext.)\s*\d{2,5})?  # extension    )''', re.VERBOSE) 

# 
someRegexValue = re.compile('foo', re.IGNORECASE | re.DOTALL)

```

