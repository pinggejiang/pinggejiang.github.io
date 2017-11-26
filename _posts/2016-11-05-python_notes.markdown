---
layout:     post
title:      "Quick start of Python for beginners (with programming background)"
subtitle:   ""
date:       2016-11-05 17:53:10
author:     "Pingge Jiang"
header-img: "img/home-bg.jpg"
catalog:    true
categories: research
tags:
    - [coding, python, tutorial]

---
Python is one of the most powerful but also easy to learn programming languages. After this tutorial, you will be able to write python projects by yourself! Good luck!

## Let's begin with a simple Python code:

```python
value = 23
speed = value
my_string = "test"

if value > 20 and value < 100
    value += 1
    my_string = my_string + "the program"

print value
print my_string
```

In Python, everything is an object. So line 2 is not a copy process, Python just tag the address of 23 with speed along with value.

Line 5 to Line 7 is a "block", defining the if  statement.

## Now let's start with details of Python:

**There are two fundamental Datatypes in Python:**

1. **Mutable**: State can be changed after creation
* ex. _list_: A = [3.24, 1000, 'foo']      
   ---------defined by bracket
* ex. _dictionary_: A = {'age': 34, 'gender': 'female'}      
   ---------defined by curly braces
2. **Immutable**: State cannot be changed after creation
* ex. _tuple_: A = (32, 'bar', 32.2)   
   ---------defined by parenthesis
* ex. _str_: A = "hello world!"
  ---------defined by ' ' or " " or "" ""
(If mutable, then you would be able to change the value stored in memory)

(Strings, tuples and lists are three sequence types in python, which can be indexed and iterable)

### strings:

```python
>>my_string = "hello world"
>> 'wor' in my_string
True

>> my_string[1:4]
'ell'

>> var = "hello" + " " + "world"
>>print var
hello world

>> var.upper()
'HELLO WORLD'

>> "data1, label1, data2, label2".split(".")
['data1', 'label1', 'data2', 'label2']

>> speed = 10
>>fuel = 5.23
>> bar = "speed = %i, fuel = %f" %(speed, fuel)
'speed = 10, fuel = 5.230000'
```
Notes of strings from above code:
<br /> * the 'in' keyword is Python style logical comparison
<br /> * sequence types in python (strings, tuples and lists) can be indexed
<br /> * strings has built-in functions: upper, split. etc.
<br /> * print out a string by using '%' sign to specify argument values

### lists:

```python
>>my_list = [43, "some words", 10, 1.23]
>>print my_list
[43, 'some words', 10, 1.23]

>>print my_list[0]
43

>>print my_list[-1]
1.23

>>my_list.append(40)
>>print my_list
[43, 'some words', 10, 1.23, 40]

>>my_list.remove(10)
>>print my_list
[43, 'some words', 1.23, 40]

>>my_list2 = [23, 40, 100, 21, 1, 59]
>>my_list2.sort()
>>print my_list2
[1, 21, 23, 40, 59, 100]

>>my_list2.reverse()
>>print my_list2
[100, 59, 40, 23, 21, 1]
```
Notes of lists from above code:
<br /> * lists in Python can store any type of values, e.g. int, float, string, char. etc.
<br /> * there are some built-in functions in Python for lists: append, remove, sort, reverse. etc.

lists has a built-in function called iterators:
```python
>>my_list = [98, 23, 'time', 32.99]
>>my_list_iter = iter(my_list)
>>my_list_iter.next()
98

>>my_list_iter.next()
23

>>my_list_iter.next()
'time'
...

>>for item in my_list:               
	------------for-loop secretly calls iter and iterator.next()
       print item
98
23
'time'
32.99
```

Notes of lists from above code:
<br /> * 'iter' is a built-in function in python for lists, every single call of 'next' will return next element in the list
<br /> * 'for ... in ...' is Python style of for-loop,  but traditional for-loop is also acceptable in Python

### tuples:
```python
>>test = (8, 23, 99, 4, 61)
>>test[2] = 37
some error                
       --------------tuples are immutable
>>test.append(690)
some error

unpacking tuples:
>>employee = ('bob', 'male', 42, 'engineer')
>>name, sex, age, job = employee
>>print name, sex, age, job
bob male 42 engineer
```
Notes of tuple from above code:
<br /> * tuples can be regarded as 'immutable lists', some built-in function of lists can be used by tuples
<br /> * element values of tuples cannot be changed. But they can be unpacked, then change the value, then pack back

we can also create a list of tuples:
```python 
>>A = [22, 33, 44, 55]
>>B = [10, 20, 30, 40]
>>C = zip(A, B)
>>print C
[(22, 10), (33, 20), (44, 30), (55, 40)]

>>for a, b in zip(A, B):
      print a, b, a+b
22 10 32
33 20 53
44 30 74
55 40 95

>>for index, (a,b) in enumerate(zip(A, B)):
      print index, a, b, a+b
0 22 10 32
1 33 20 53
2 44 30 74
3 55 40 95
```

### dictionaries:
```python
>>employee = {'jobs': 'boss', 'age': 38, 'pay':7200}
>>print employee['job']
boss

>>print employee['age']
38

>>for i in employee:
    print '%s: %s' % (str(i), str(employee[i]))
pay: 72000
job: boss
age: 38

>>employee.items()
[('pay', 72000), ('job', 'boss'), ('age', 38)]
```
Notes of dictionaries from above code:
<br /> * dictionaries are kind of 'struct' in C
<br /> * use ':' to specify class name and class value
<br /> * 'str' is a keyword in Python to convert other data types to string
<br /> * 'items' secretly calls 'for ... in ...' and return (class name, class value) pairs

## After introducing the datatypes of python, let's see some basic file I/O methods:
```python
read example:

>>f = open('test.txt')
>>f.readline()
>>f.read(5)                              --------------read 5 bytes
>>f.seek(0)                              --------------move curser to byte zero(the beginning of the file)
>>f.seek(0, os.SEEK_SET)                 --------------move to byte 0, starting from the beginning of the file
>>f.seek(-10, os.SEEK_END)               --------------move backward 10 bytes, starting from the end of the file
>>f.seek(5, os.SEEK_CUR)                 --------------move forward 5 bytes, starting from the current position

write example:

>>f = open('test.txt', 'w+')
>>f.writelines(list)
>>f.seek(0)
>>for line in f:
     print line
>>f.close()
```

## Python style of loops:

### range:

```python
>>my_list = range(5)
>>print my_list
[0, 1, 2, 3, 4, 5]

>>my_list = range(15, 20)
>>print my_list
[15, 16, 17, 18, 19]

>>my_list = range(15, 30, 3)
>>print my_list
[15, 18, 21, 24, 27]
```
Notes of range() from above code:
<br /> * standard format: range(start, end, interval)
<br /> * if start value is not provided, default start is 0
<br /> * return arrays are [start, end), which means  start <= values < end

### for-loop:
```python
my_list = [8, 23, 99, 4, 61]

for i in range(len(my_list)):
    item = my_list[i]
    foo = item * 10 + 4
    print foo
```
Notes of for-loop from above code:
<br /> * 'len' is a keyword to get the length of lists
<br /> * Any scope in Python (for loops, if statements, class definitions, etc.) are defined in 'blocks', defined by spaces  and comma instead of 'end' keyword

### enumerate:
```python
>>my_list = [8, 23, 99, 4, 61]
>>foo = enumerate(my_list)
>>foo.next()
(0, 8)                -----------tuple
>>foo.next()
(1, 23)               -----------tuple

for index, item in enumerate(my_list):
      my_list[index] *= 10
      print my_list
...
```
Notes of enumerate from above code:
<br /> * 'enumerate' function produces (index, value) pairs
<br /> * for-loop in Python can extract multiple items from input arguments

### list comprehension:
```python
[expression for name in list]
ex:
>>my_list = [10, 20, 30, 40, 50]
>>new_list = [item**2 for item in my_list]
>>print new_list
[100, 400, 900, 1600, 2500]
```

## With all the backgrounds, let see some more complicated Python codes:
```python
alpha = 0.24                                     -----------global variable

def my_function(parameter):                      -----------definition name
    age = 34
    radius = 100                                 -----------definition body
    delta = parameter * alpha      
    return age * radius * delta                  -----------return value

def my_range(start, stop, step=1)                -----------input parameter can also has default value
    numbers = []

result = my_function(2)
print result
```
Notes from above code:
<br /> * variables defined outside of definitions are global variables
<br /> * variables defined inside of definitions are local variables and cannot be used outside of definition
<br /> * 'def' is the keyword if define a definition

## import libraries or other functions:
```python
import example                              -------------import whole file

total = 0
for item in example.my_range(2, 21, 2):     --------------my_range is in example.py
     total += item
print total

or:

from example import my_range                --------------import single definition from a file

total = 0
for item in my_range(2, 21, 2):
    total += item

or:

from example import my_range as flower      ---------------import single definition from a file with alias

total = 0
for item in flower(2, 21, 2):
    total += item
```

## import matlab plot library to plot images:
```python
from matplotlib import pyplot as plt
from math import sin, pi

sine_wave = [sin(2*pi*x*0.001) for x in xrange(0,1000)]

plt.plot(sine_wave)
plt.show()
```
## finally, classes in Python: leave details for you to google :)
![python](/img/post/python_notes.png)
