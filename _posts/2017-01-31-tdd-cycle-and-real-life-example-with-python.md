---
layout: post
title: TDD cycle and real life example with python
date: 2017-01-31 23:12 -0400
categories: [Python, Unit Test, Agile, TDD]
cover: tdd_cycle.jpg
cover_copyright: "xsjjys.com"
author: abe_kroenem
---

#### Combine problem solving, scalability, fail-safe development and make the code work for you.
------------------------------------------------------------------------------------------------


__TDD (Test-Driven Development)__ is an methodology to build applications by thinking about desired results first, 
coveraging it with tests, which describing what are the steps your program/feature must follow to archieve these results, so that later, you write production code as you usually do.

The TDD clycle, have 3 simple main stages, <span class='red'>__Test Fails__</span>, <span class='green'>__Test Passes__</span> and <span class='blue'>__Code Refactor__</span>.

* In <span class='red'>Test Fails</span> stage, we primarily think about result. 
We just write code enumerating how we can reach the result with every single part of our code lines, thats include 
objects, classes, interfaces and etc.

* In <span class='green'>Test Passes</span> stage, we write the tests resolutions.
Here, the main code of our software is written. Every class, object, routes, database persistence gains life.

* And, in the last stage, <span class='blue'>Code Refactor</span>, we make an precisely read of our code.
We search for broken rules in standard recommendations, redundancy, unecessary complexity and confusing scopes, and then, perform a general refactoring.
Moreover, write some [Documentation Blocks](https://medium.freecodecamp.org/code-comments-the-good-the-bad-and-the-ugly-be9cc65fbf83) it's a good idea.

Then, let's analyse a simple use case, and try to apply this cycle:

__1. Write a algorithm which allow the user to create products.__
<br>
__2. Every product, when it be created, must have at least an quantity avaliable in stock.__

It's pretty simple, right? 

Independent of the environment you use to write code, a probable solution have crossed your head.

So, my implementation, with __python__ and [__unittest__](https://docs.python.org/2/library/unittest.html) library, is the following:


```python
# app.py
"""
unittest library
will perform the tests suite
"""
import unittest 

"""
our test case class
it will hold all the methods with the tests implementations
"""
class ProductTestCase(unittest.TestCase):
    
    """
    testing requirement 2: 
        'must have at least one avaliable to be consumed'
    """
    def test_product_create(self):

        product = Product(name='Soda', price=15.0)

        # checking the desired behaviour
        self.assertEquals(product.stock, 1)


# Placed here to start testing our file
if __name__ == '__main__':
    unittest.main()
```

Great, so i do run and this was the result:

```shell
$ python app.py
E
======================================================================
ERROR: test_product_create (__main__.ProductTestCase)
----------------------------------------------------------------------
Traceback (most recent call last):
File "app.py", line 6, in test_product_create
    product = Product(name='Soda', price=15.0)
NameError: global name 'Product' is not defined

----------------------------------------------------------------------
Ran 1 test in 0.000s

FAILED (errors=1)
```

At this point, we ensure that the first step <span class='red'>__Test Fails__</span> appears.

The output said to us:

```
Ran 1 test in 0.000s

FAILED (errors=1)
```

And, why it fails:

```
NameError: global name 'Product' is not defined
```

Cool, right then, let's create our `Product` class:

__Note:__ Now, we are creating ordinary code.

```python
# app.py
"""
unittest library
will perform the tests suite
"""
import unittest 

class Product:

    def __init__(self, name, price):
        self.name = name
        self.price = price
        self.stock = 1 # Here, our demand is met

"""
our test case class
it will hold all the methods with the tests implementations
"""
class ProductTestCase(unittest.TestCase):
    
    """
    testing requirement 2: 
        'must have at least one avaliable to be consumed'
    """
    def test_product_create(self):

        product = Product(name='Soda', price=15.0)

        # checking the desired behaviour
        self.assertEquals(product.stock, 1)


# Placed here to start testing our file
if __name__ == '__main__':
    unittest.main()
```

Running it again:

```shell
$ python app.py
.
----------------------------------------------------------------------
Ran 1 test in 0.000s

OK
```

Finally, our test suite work as expected. The <span class='green'>__Test Passes__</span> stage appears.

__Well, eveything it's done now, right? I can upload it to my production server and take my fee from the client.__

__Hurr ... I don't think so.__

This last lines implemented in our `app.py` file, have a class definition.
<br>
An often problem i saw here was:

#### Multiples responsabilities !!##

Well, the `app.py` take care about two individual layers, __Model__ and __TestCase__.

The good know pratice about design patterns and software architecture, such as [MVC](https://medium.freecodecamp.org/model-view-controller-mvc-explained-through-ordering-drinks-at-the-bar-efcba6255053),
defines that decoupling of classes and parts of code create a more maintanable envirioment.
By thinking in growth possibility, a single file will just make growth
our wish to delete everything and start all again.

Then, let's split our single-app file in two.

Models definition: 

```python
# models.py

class Product:

    def __init__(self, name, price):
        self.name = name
        self.price = price
        self.stock = 1 # Here, our demand is met
```

And test suites:

```python
# test.py
"""
unittest library
will perform the tests suite
"""
import unittest 
from models import Product

"""
our test case class
it will hold all the methods with the tests implementations
"""
class ProductTestCase(unittest.TestCase):
    
    """
    testing requirement 2: 
        'must have at least one avaliable to be consumed'
    """
    def test_product_create(self):

        product = Product(name='Soda', price=15.0)

        # checking the desired behaviour
        self.assertEquals(product.stock, 1)


# Placed here to start testing our file
if __name__ == '__main__':
    unittest.main()
```

Now, a teaste of <span class='blue'>__Code Refactor__</span> stage shows up.

All of code definitions have your place in the software.

Models in `model.py` file and TestCases in `test.py` file. 

Great, by running it again, to make sure everything still working properly:

```shell
$ python test.py
.
----------------------------------------------------------------------
Ran 1 test in 0.000s

OK
```

And there it is. The 3 stages reached, the solution given and a scalable structure started. Here, at this point, we guarantee that this part of our implementation is working and good to go.

This was just a small perspective of what TDD can offer to your software.

__Most of best pratices programming articles, talk about TDD acceptance and all of his benefits__ to have it included in our stack and
I see no reason why do not use TDD when possible.

Read more:
* [Agile Software Programming Best Pratices (versionone.com)](https://www.versionone.com/agile-101/agile-software-programming-best-practices/)
* [Best Pratices Software Development and Testing (opensource.com)](https://opensource.com/article/17/5/30-best-practices-software-development-and-testing)
* [Why is Test-Driven Development Useful? (medium.com)](https://medium.com/@fagnerbrack/why-test-driven-development-4fb92d56487c)