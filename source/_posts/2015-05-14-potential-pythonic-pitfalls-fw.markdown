---
layout: post
title: "Potential Pythonic Pitfalls(FW)"
date: 2015-05-14 09:24:13 +0800
comments: true
categories: python
keywords: Pythonic, Pitfalls
description: Potential Pythonic Pitfalls
---
Python is a very expressive language. It provides us with a large standard library and many builtins to get the job done quickly. However, many can get lost in the power that it provides, fail to make full use of the standard library, value one liners over clarity and misunderstand its basic constructs. This is a non-exhaustive list of a few of the pitfalls programmers new to Python fall into.<!--more-->

##Not Knowing the Python Version
This is a recurring problem in StackOverflow questions. Many write perfectly working code for one version but they have a different version of Python installed on their system.<sup>[1]</sup> Make sure that you know the Python version you're working with. You can check via the following:

	$ python --version
	Python 2.7.9

##Not using Pyenv
[pyenv](https://github.com/yyuu/pyenv) is a great tool for managing different Python versions. Unfortunately, it only works on *nix systems. On Mac OS, one can simply install it via  ```brew install pyenv```  and on Linux, there is an [automatic installer](https://github.com/yyuu/pyenv-installer).

##Obsessing Over One-Liners
Some get a real kick out of one liners. Many boast about their one-liner solutions even if they are less efficient than a multi-line solution.

What this essentially means in Python is convoluted comprehensions having multiple expressions. For example:

	l = [m for a, b in zip(this, that) if b.method(a) != b for m in b if not m.method(a, b) and reduce(lambda x, y: a + y.method(), (m, a, b))]

To be perfectly honest, I made the above example up. But, I've seen plenty of people write code like it. Code like this will make no sense in a week's time. If you're trying to do something a little more complex that simply adding an item to a ```list``` or ```set``` with a condition then you're probably making a mistake.

One-Liners are not achievements, yes they can seem very clever but they are not achievements. Its like thinking that shoving everything into your closet is an actual attempt at cleaning your room. Good code is clean, easy to read and efficient.

##Initializing a set the Wrong Way
This is a more subtle problem that can catch you off guard. ```set``` comprehensions are a lot like list comprehensions.

	>>> { n for n in range(10) if n % 2 == 0 }
	{0, 8, 2, 4, 6}
	>>> type({ n for n in range(10) if n % 2 == 0 })
	<class 'set'>

The above is one such example of a set comprehension. Sets are like lists in that they are containers. The difference is that a set cannot have any duplicate values and sets are unordered. Seeing set comprehensions people often make the mistake of thinking that ```{}``` initializes an empty set. It does not, it initializes an empty dict.

	>>> {}
	{}
	>>> type({})
	<class 'dict'>

If we wish to initialize an empty set, then we simply call ```set()```.

	>>> set()
	set()
	>>> type(set())
	<class 'set'>

Note how an empty set is denoted as ```set()``` but a set containing something is denoted as items surrounded by curly braces.

	>>> s = set()
	>>> s
	set()
	>>> s.add(1)
	>>> s
	{1}
	>>> s.add(2)
	>>> s
	{1, 2}

This is rather counter intuitive, since you'd expect something like ```set([1, 2])```.

##Misunderstanding the GIL
The GIL (Global Interpreter Lock) means that only one thread in a Python program can be running at any one time. This implies that when we create a thread and expect to run in parallel it doesn't. What the Python interpreter is actually doing is quickly switching between different running threads. But this is an oversimplified version of what is actually happening. There are many instances in which things do run in parallel, like when using libraries that are essentially C extensions. But when running Python code, you don't get parallel execution most of the time. In other words, threads in Python are not like Threads in Java or C++.

Many will try to defend Python by saying that these are real threads<sup>[2]</sup>. This is indeed true, but does not change the fact that how Python handles threads is different from what you'd generally expect. This is the same case for a language like Ruby (which also has an interpreter lock).

The prescribed solution to this is using the ```multiprocessing``` module. The ```multiprocessing``` module provides you with the ```Process``` class which is basically a nice cover over a fork. However, a fork is much more expensive than a thread, so you might not always see the performance benefits since now the different processes have to do a lot of work to co-ordinate with each other.

However, this problem does not exist every implementation of Python. [PyPy-stm](http://pypy.readthedocs.org/en/latest/stm.html) for example is an implementation of Python that tries to get rid of the GIL (still not stable yet). Implementations built on top of other platforms like the JVM (Jython) or CLR (IronPython) do not have GIL problems.

All in all, be careful when using the ```Thread``` class, what you get might not be what you expect.

##Using Old Style Classes
In Python 2 there are two types of classes, there's the "old style" classes, and there's the "new style" classes. If you're using Python 3, then you're using the "new style" classes by default. In order to make sure that you're using "new style" classes in Python 2, you need to inherit from ```object``` for any new class you create that isn't already inheriting from a builtin like ```int``` or ```list```. In other words, your base class, the class that isn't inheriting from anything else, should always inherit from object.

	class MyNewObject(object):
	    # stuff here

These "new style" classes fix some very fundamental flaws in the old style classes that we really don't need to get into. However, if anyone is interested they can find the information in the [related documentation](https://docs.python.org/2/reference/datamodel.html#new-style-and-classic-classes).

##Iterating the Wrong Way
Its very common to see the following code from users who are relatively new to the language:

	for name_index in range(len(names)):
	    print(names[name_index])

There is no need to call ```len``` in the above example, since iterating over the list is actually much simpler:

	for name in names:
	    print(name)

Furthermore, there are a whole host of other tools at your disposal to make iteration easier. For example, ```zip``` can be used to iterate over two lists at once<sup>[3]</sup>:

	for cat, dog in zip(cats, dogs):
	    print(cat, dog)

If we want to take into consideration both the index and the value list variable, we can use ```enumerate```:<sup>[4]</sup>

	for index, cat in enumerate(cats):
	    print(cat, index)

There are also many useful functions to choose from in [itertools](https://docs.python.org/3/library/itertools.html). Please note however, that using ```itertools``` functions is not always the right choice. If one of the functions in ```itertools``` offers a very convenient solution to the problem you're trying to solve, like flattening a list or creating a getting the permutations of the contents of a given list, then go for it. But don't try to fit it into some part of your code just because you want to.

The problem with ```itertools``` abuse happens so often that one highly respected Python contributor on StackOverflow has dedicated a significant part of their profile to it.<sup>[5]</sup>

##Using Mutable Default Arguments
I've seen the following quite a lot:

	def foo(a, b, c=[]):
	    # append to c
	    # do some more stuff

Never use mutable default arguments, instead use the following:

	def foo(a, b, c=None):
	    if c is None:
	        c = []
	    # append to c
	    # do some more stuff

Instead of explaining what the problem is, its better to show the effects of using mutable default arguments:

	In[2]: def foo(a, b, c=[]):
	...     c.append(a)
	...     c.append(b)
	...     print(c)
	...
	In[3]: foo(1, 1)
	[1, 1]
	In[4]: foo(1, 1)
	[1, 1, 1, 1]
	In[5]: foo(1, 1)
	[1, 1, 1, 1, 1, 1]

The same ```c``` is being referenced again and again every time the function is called. This can have some very unwanted consequences.

##Takeaway
These are just some of the problems that one might run into when relatively new at Python. Please note however, that this is far from a comprehensive list of the problems that one might run into. The other pitfalls however are largely to do with people using Python like Java or C++ and trying to use Python in a way that they are familiar with. So, as a continuation of this, try diving into things like Python's ```super``` function. Take a look at ```classmethod```, ```staticmethod``` and ```__slots__```.

##Update
Last Updated on 12 May 2015 4:50 PM (GMT +6)

Made the section on [Misunderstanding the GIL](http://nafiulis.me/potential-pythonic-pitfalls.html#misunderstanding-the-gil)

--------
[1]	Most people are taught Python using Python 2. However, when they go home and try things out themselves, they install Python 3 (quite a natural thing to install the latest version).  
[2]	When people talk about real threads what they essentially mean is that these threads are real CPU threads, which are scheduled by the OS (Operating System).  
[3]	[https://docs.python.org/3/library/functions.html#zip](https://docs.python.org/3/library/functions.html#zip)  
[4]	```enumerate``` can be further configured to produce the kind of index you want. [https://docs.python.org/3/library/functions.html#enumerate](https://docs.python.org/3/library/functions.html#enumerate)  
[5]	[http://stackoverflow.com/users/908494/abarnert](http://stackoverflow.com/users/908494/abarnert)

[Original](http://nafiulis.me/potential-pythonic-pitfalls.html)
