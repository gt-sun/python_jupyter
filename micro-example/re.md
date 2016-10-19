[TOC]

### tips

- `group(0)` 0表示整个匹配的字符串，默认为0；
- `groups()` 以元祖形式返回全部匹配的字符串，相当于`group(1,2,3,...)`

### 匹配中文

```python
....
re.findall(u'([\u4e00-\u9fa5]+)', text)
....
```

### re.compile

```python
import re

p = re.compile(r'hello')
m = p.match('hello world!')

if m is not None:
    print(m.group())
```

### re.split

```python
p = re.compile(r'\d+')
print(p.split('one1two2three3four4'))  # ['one', 'two', 'three', 'four', '']
```

### re.sub

- 两两交换位置

```python
import re
 
p = re.compile(r'(\w+) (\w+)')
# s = 'i say, hello world!'
s = 'a b c d'
 
print(p.sub(r'\2 \1', s))  # b a d c
```
