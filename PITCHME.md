#HSLIDE
<img src="http://www.cakex.org/sites/default/files/National_Conservation_Training_Center.gif" width="200">
<img src="https://www.python.org/static/img/python-logo.png" width="400">
#### Python for Advanced GIS
<br>
<span style="color:gray">NCTC Advanced GIS</span>
<br>
<span style="color:gray">Daryl Van Dyke</span>

#HSLIDE
## Major Language Elements
* Tuples, Dictionaries, Sets, and Iterables
* Classes
* Modules and Reusable code

#HSLIDE
## Tuples, Dictionaries, Sets and Iterables
* Dictionaries and sets are two other flavors of iterable data containers.

* Sets are **mutable**, unordered, and guarenteed to have unique Elements.

* Dictionaries are a way of storing "Key-Value" pairs.

#HSLIDE
## Tuples
* `tuple` is an *immutable* data structure
* They are created, and cannot be changed.
* They use parantheses `(objA, objB)`
* Static Information Storage in a Structured Form

#HSLIDE
## Tuples - Point Data Model
*  Commonly, I use them when reading stuff in and making a cheap data model
```python
>>> lat1 = 15.32
>>> lat2 = 44.3
>>> lon1 = 17.2
>>> lon2 = 23.4
>>> elev1 = 1230.3
>>> elev2 = 834.3
```

#HSLIDE
## Tuples - Point Data Model
```python
>>> point1 = (lat1, lon1, elev1)
>>> point2 = (lat2, lon2, elev2)
>>> listPoints = []
>>> listPoints.append(point1)
>>> listPoints.append(point2)
```

#HSLIDE
## Tuples - Point Data Model
```python
>>> # average Z
>>> sumZ = 0.
>>> numPoints = 0L
>>> for point in listPoints:
	thisZ = point[2] #grab the Z
	sumZ = sumZ + thisZ
	numPoints = numPoints + 1
	# numPoints += 1  - alternate increment
```
#HSLIDE
## Tuples - Average Z
```python
>>> sumZ
2064.6
>>> numPoints
2L
>>> averageZ = sumZ/numPoints
>>> averageZ
1032.3
```

#HSLIDE
## Dictionaries
* Useful for storing objects when we want to be able to retrieve them by 'name'
* Instead of referencing (retrieving) an element by *index number*, we give them a name.
* `key` can be any **immutable** type: notably, string, numbers, and tuples

#HSLIDE
## Dictionaries - A Telephone Dictionary
```python
>>> tel = {'jack': 4098, 'sape': 4139}
>>> tel['guido'] = 4127
>>> tel
{'sape': 4139, 'guido': 4127, 'jack': 4098}
>>> tel['jack']
4098
```
#HSLIDE
## Dictionaries - A GIS example
```python
>>> buffDist = {}
>>> buffDist["chupacabra"] = 100.3
>>> buffDist["samsquatch"] = 134.3
>>> buffDist["nautilus"] = 117.45e24
>>> buffDist
{'chupacabra': 100.3, 'nautilus': 1.1745e+26, 'samsquatch': 134.3}
```

#HSLIDE
## Dictionaries - A GIS example
```python
>>> buffDist
{'chupacabra': 100.3, 'nautilus': 1.1745e+26, 'samsquatch': 134.3}
>>> critterES = "chupacabra"
>>> arcpy.Buffer_analysis(inputFC, inputFC+"Buf"+critterES,
		          buffDist[critterES])
```
#HSLIDE
## Sets
*Sets* are a python data type that are:
* unordered
* and guarenteed to be duplicate-Free.

Very useful for making a 'list' as you iterate through that can have not repetitions.

#HSLIDE
## Sets
```python
>>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
>>> fruit = set(basket)               # create a set without duplicates
>>> fruit
set(['orange', 'pear', 'apple', 'banana'])
>>> 'orange' in fruit                 # fast membership testing
True
>>> 'crabgrass' in fruit
False
```

#HSLIDE
## LPT: Unique-Element List
### How many non-unique elemens?
```
>>> listFromProcess = [343, 646, 26, 5775, 646, 984, 34]
>>> numAll = len(listFromProcess)
>>> listUnique = list(set(listFromProcess))
>>> numRepeats = numAll - len(listUnique)
>>> numRepeats
1
```

#HSLIDE
##  Objects and Classes
All Python 'things' are **instances** of an object.
A **Class** is a blueprint for a data model.  It defines a 'thing' that has:
* usually has an initilization (`__init__`) method
* additional methods (functions bound to the object)
* and attributes ("slots" that hold data, and are addressable by name.

#HSLIDE
## Classes and Objects
When we want to make one of these, we "instantiate it",
or "make an instance".

A class can have attributes (variables) that are unique to that instance,
or it can have variables shared by all instances of that class.

#HSLIDE
## Class & Object Nomenclature
|Class Element | Translation |
-----------------------------------------------
|class 'variable'  | class attribute |
|class 'function'    | class 'function'|
|instance 'variable' | an object's unique attributes|
|instance 'function' | an object's method, almost always common to all instances.|

#HSLIDE
## Why Classes?
Classes are blueprints, or ***data models***
They allow us to organize code in a way that makes logical sense.
Classes are ***awesomesauce***.

The classic tests are 'is a' and 'has a'.

'is a ' -->  class instance

'has a ' --> class attribute

#HSLIDE
## UML (Universal Markup Language)
There is a standard way of drawing ('marking up') classes.
This should be awesome, a way of using the graphic tools to model information and then generate code.
I haven't seen it work that way yet, but probably my fault.

#HSLIDE
## UML (Universal Markup Language)
<img src="http://agilemodeling.com/images/models/classDiagramStudentAddress.jpg">

#HSLIDE
So - the classic example.  Things that are dogs.

#HSLIDE
### The Dog Class
```python
class Dog:

    kind = 'canine'         # class variable shared by all instances

    def __init__(self, name):
        self.name = name    # instance variable unique to each instance
```
#HSLIDE
## The ```__init__``` method:
```python
class Dog:

    kind = 'canine'         # class variable shared by all instances

    def __init__(self, name):
        self.name = name    # instance variable unique to each instance
```
- Always called when an object is instantiated
- Always has *at least one* argument (***self***)
- Is how we pass arguments into the creation of the class.

#HSLIDE
## The ```__init__``` method:
***Import concept:***
Defining what you need for ```__init__``` is a critical part of the data modeling.
Think carefully.

#HSLIDE
```python
class Dog:
    kind = 'canine'         # class variable shared by all instances
    def __init__(self, name):
        self.name = name    # instance variable unique to each instance

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
```
#HSLIDE
So now we have two instances of the Dog Class.
One called (name-bound) to ```d```, the other to ```e```.

#HSLIDE
# Class Variables vs. Instance Variables
```python
>>> d.kind                  # shared by all dogs
'canine'
>>> e.kind                  # shared by all dogs
'canine'
>>> d.name                  # unique to d
'Fido'
>>> e.name                  # unique to e
'Buddy'
```
#HSLIDE
## Careful with those class variables...
```python
class Dog:

    tricks = []             # mistaken use of a class variable

    def __init__(self, name):
        self.name = name

    def add_trick(self, trick):
        self.tricks.append(trick)

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.add_trick('roll over')
>>> e.add_trick('play dead')
>>> d.tricks                # unexpectedly shared by all dogs
['roll over', 'play dead']
```
#HSLIDE
## You probably wanted this...
```python
class Dog:

    tricks = []             # mistaken use of a class variable

    def __init__(self, name):
        self.name = name

    def add_trick(self, trick):
        self.tricks.append(trick)

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.add_trick('roll over')
>>> e.add_trick('play dead')
>>> d.tricks                # unexpectedly shared by all dogs
['roll over', 'play dead']
```
#HSLIDE
## Too much for now...
File these away:
***Class inheritance:***
If Fido 'is a' Dog, and a Dog 'is a' Mammal, and a Mammal 'is an' Animal.
Can we do that with classes?  Oh sure!

#HSLIDE
```python
class Animal:
    def __init__(self, strSpineType)
        self.spine = strSpineType
    def move(self, moveType):
        self.move = "unknown"
        print "I don't know!"
Class Dog(Animal):
    # here, the move method 'overrides' the base class one.
    #Cool, huh?
    def move(self, moveType):
        i = 10
        while i > 0:
          self.move = "crawl"
          i = i - 1
```
#HSLIDE
## Name Mangling
In a nutshell, in Java we can 'lock down' methods and attributes so they are *unavailable* outside the class.
Python doesn't support that, we have a convention.  If something is **not** to be messed with outside of the class, use an underscore up front.
`self._numWidgets`
This means that 'the object manages this attribute, don't mess with it.

#HSLIDE
## The Object as a Bucket
"Gosh Daryl, that seems complicated.  What if I just want a container to dump information into?"
```python
class Dog:
    pass  #remember pass?  It allows an empty block

fido = Dog()  #tell me about this dog...
fido.fleas = False
fido.food = "hungry"
fido.legs = 3
```
