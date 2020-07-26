# Classes and Objects

Python supports all OOP features, such as abstraction, inheritance, polymorphism, and encapsulation.

Built-in data structure and collections can be helpful in maintaining a simple state. But for managing more complicated states Classes are better tools. But that's not the only fact about the Classes.

In OOP and Domain-driven design approach the Objects should handle the custom behavior. Classes can encapsulate both the behavior and the state.

<br>

## Definition

Classes can simply defined as below:

```py
class Employee:
    pass
```

In order to use a class we need to instantiate it:

```py
it_manager = Employee()
```

Note that unlike other c-type languages there is no need to use the ```new``` keyword.

In Python, it's possible to define instance variables directly on the instance:

```py
it_manager.firstname = 'John'
```

This variable is only owned by the ```it_manager``` instance. <br>
We can confirm this by using ```__dict__```:

```py
print(Employee.__dict__)
print(it_manager.__dict__)
```

```Class``` should act as a blueprint for the objects instantiated from that class.
Therefore, chances are that we prefer to define variables inside the class:

```py
class Employee:
    firstname = ''
```

Variables are states of an object. Methods add behavior to the object:

```py
class Employee:
    firstname = 'Jane'
    lastname = 'Doe'

    def __init__(self):
        pass

    def get_fullname(self):
        return '{first} {last}'.format(first = Employee.firstname, last = Employee.lastname)
    
    def add_task(self, title):
        pass
```

```__init__``` is the constructor of the class and runs when the class is being instantiated. <br>
Though the ```pass``` keyword used here indicates the compiler to pass on this method. ```pass``` is used when we want to define a method but would not want to implement it.

```self``` is a pointer to the current instance of the class. Similar to ```this``` keyword in other OOP languages.<br> It can be called anything but the convention is name it *self*.
The difference here is that it has to be included as a parameter on every function.

The important thing to note is that we cannot access the variables inside a class as:

```py
def get_fullname(self):
    return firstname + ' ' + lastname
```

The variables are either ```Class``` or ```Instance``` variable.

You define class variables as:

```py
def get_fullname(self):
    return Employee.firstname + ' ' + Employee.lastname
```

```py
def get_fullname(self):
    return self.firstname + ' ' + self.lastname
```

So, in the previous example which was using the class variable, the instance cannot change the outcome:

```py
it_manager = Employee()
it_manager.firstname = 'John'

print(it_manager.get_fullname())
```

The output will be:

```bash
Jane Doe
```

But if we had written the code as:

```py
class Employee:
    firstname = 'Jane'
    lastname = 'Doe'

    def __init__(self):
        pass

    def get_fullname(self):
        return '{0} {1}'.format(self.firstname, self.lastname)
    
    def add_task(self, title):
        pass

it_manager = Employee()
it_manager.firstname = 'John'

print(it_manager.get_fullname())
```

The output would have been:

```bash
John Doe
```

But there are use cases for Class level variables, such as setting and persisting a value whenever the class was instantiated.

Now, of course the better practice is to pass parameters to the constructor/methods instead of hard-coding.

```py
class Employee:
    firstname = ''
    lastname = ''

    def __init__(self, firstname, lastname):
        self.firstname = firstname
        self.lastname = lastname

    def get_fullname(self):
        return '{0} {1}'.format(self.firstname, self.lastname)

    def add_task(self, title):
        pass


it_manager = Employee('John', 'Doe')
developer = Employee('Jane', 'Doe')


print(it_manager.get_fullname())
print(developer.get_fullname())
```

<br>

## Static methods

Instance methods take the instance (self) as the default first argument.

Class or Static methods should be defined like:

```py
@classmethod
def method_name(cls, param1):
    pass
```

```@classmethod``` is a decorator, and ```cls``` - named by convention - is pointing to the Class.

The class method can then be used to call from the Class and it will affect all the instances: 

```py
class Employee:
    firstname = ''
    lastname = ''
    monthly_bonus = 0.0

    def __init__(self, firstname, lastname):
        self.firstname = firstname
        self.lastname = lastname

    def get_fullname(self):
        return '{0} {1}'.format(self.firstname, self.lastname)

    def add_task(self, title):
        pass

    @classmethod
    def set_bonus(cls, amount):
        cls.monthly_bonus = amount

it_manager = Employee('John', 'Doe')

Employee.set_bonus(1000)

print (Employee.monthly_bonus) # 1000
print (it_manager.monthly_bonus) # 1000
```

Unlike most of other OOP languages Static methods can be call on the instance objects as well.

<br>

## Special methods

We can perform **operator overloading** to classes with special methods - also called *magic* or *dunder* methods.

To elaborate, ```+``` sign will perform an *Addition* with Integer types and *Concatenation* with Strings. That is because this operator behavior has been modified in each of these classes.

Special methods which are useful to every class defined are: ```__repr__``` and ```__str__```.

```py
class Employee:

    def __init__(self, firstname, lastname):
        self.firstname = firstname
        self.lastname = lastname

    def get_fullname(self):
        return '{0} {1}'.format(self.firstname, self.lastname)

    def __repr__(self):
        pass

    def __str__(self):
        pass
```

```__repr__``` is for developers and needs to be unambiguous. ```__str__``` is for the users and should be more readable.

```py
class Employee:

    def __init__(self, firstname, lastname):
        self.firstname = firstname
        self.lastname = lastname

    def get_fullname(self):
        return '{0} {1}'.format(self.firstname, self.lastname)

    def __repr__(self):
        return "Employee('{}', '{}')".format(self.firstname, self.lastname)

    def __str__(self):
        return "{} - {}".format(self.firstname, self.lastname)

emp_01 = Employee('Jane', 'Doe')

print(emp_01.__repr__()) # Employee('Jane', 'Doe')
print(emp_01.__str__()) # Jane - Doe
```

Other special methods includes ```__add__```, ```__len__```, etc.

```py
def __add__(self, other):
    self.bonus + other.bonus
```