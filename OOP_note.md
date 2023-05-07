# Some Notes of Learning Object-oriented Programming

### Main goal of OOP: model real-world entities into software-object

___

#### Class and Object

* Class is a blueprint or a template for creating objects.
* Class defines the attributes and methods that objects will have.
* An object is an instance of a class. It contains data and methods that are defined in the Class.

```python
class Person:
    # __init__ method is known as Constructor
    def __init__(self, name, height):
        self.name = name
        self.height = height

    def get_name(self):
        return self.name

    def get_height(self):
        return self.height
```

* creating a simple class with attribute and method

```python
p = Person("Patrik", 174)
print(f"{p.get_name()} is {p.get_height()} cm tall.")
# output : Patrik is 174 cm tall.
```

* creating an object of the Person class and accessing its attributes.

___

#### Main features of OOP:

* ##### Inheritance :

Creating a new class from an existing class.
The new class will inherit the attributes and methods of existing class.
The class Dog inherited the attribute:name of class Animal and overriding the method.
This **method overriding** achieved another feature: Polymorphism.

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        return f"{self.name} says : I am an Animal"


class Dog(Animal):
    def speak(self):
        return "Woof!"


class Cat(Animal):
    def speak(self):
        return "Meow~"


sheep = Animal("Sheep") # better to create a Sheep concrete class and treat Animal as an abstract not-instantiatable class
dog = Dog("Lucky")
cat = Cat("Kitty")

print(sheep.name, sheep.speak())  # output : Sheep Sheep says : I am an Animal
print(dog.name, dog.speak())  # output : Lucky Woof!
print(cat.name, cat.speak())  # output : kitty Meow~
```

* ##### Polymorphism:

Polymorphism is achieved through method overriding and method overloading,
using a single method or attribute in different ways for different objects.
Making code easily to be expanded (reusable), read (better organization).
The **method overriding** provide a different implementation for a method in a subclass than the one in the superclass (useful when you want to change the behavior of a method in a specific subclass). The **method overriding** example is
same as above.

**Method overloading** is a way to define multiple methods with the same name but different parameters in a class
(useful to provide different ways to call a method, depending on the arguments that are passed in).
In Python, there is no built-in support for **method overloading** (simulate it by using default arguments and
variable-length arguments).The example of overloading is as following.

```python
from multipledispatch import dispatch

# not using overloading:
def save_user_into_db(username: str, password: str):
    pass
def save_user_into_db_using_dict(user_data: Dict[str, str]):
    pass

# using overloading:
@dispatch(str,str)
def save_user_into_db(username: str, password: str):
    pass

@dispatch(Dict[str,str])
def save_user_into_db(user_data: Dict[str, str]):
    pass
```

* ##### Abstraction:

An Abstract Class is like the template for other classes, the abstract methods that belongs to the abstract class 
must also be created in the subclasses. It is useful when designing a huge functional unit,e.g. create a new library, or
provide a common interface for different classes.

```python

from abc import ABC, abstractmethod
class Animal(ABC):
 
    @abstractmethod
    def speak(self):
        pass
    

class Dog(Animal):
    def speak(self):
        return "Woof!"


class Cat(Animal):
    def speak(self):
        return "Meow~"
    
class Pig(Animal):
    pass
    
 
d = Dog()
print(d.speak()) # output: Woof!

c = Cat()
print(c.speak()) # output: Meow~

p = Pig() 
print(p.speak()) # TypeError: Can't instantiate abstract class Pig with abstract method speak

```
* ##### Encapsulation


Hiding the implementation details of a class from the outside, only exposing a public interface that can be used to interact with the class.
The private attributes can be accessed with underscore, but we shouldn't access them directly.

```python
# create a class with private attributes and methods
class BankAccount:
    def __init__(self, balance):
        self._balance = balance  

    def deposit(self, amount):
        self._balance += amount

    def withdraw(self, amount):
        if amount <= self._balance:
            self._balance -= amount
        else:
            print("Insufficient balance")

    def get_balance(self):
        return self._balance

# Creating an object of the BankAccount class and accessing its public methods
b = BankAccount(1000)
b.deposit(500)
b.withdraw(200)
print(b.get_balance())  # Output: 1300
```

___