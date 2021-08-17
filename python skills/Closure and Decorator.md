# Python CLosures
> https://www.programiz.com/python-programming/closure

## Example
```
def print_msg(msg):
    # This is the outer enclosing function

    def printer():
        # This is the nested function
        print(msg)

    return printer  # returns the nested function


# Now let's try calling this function.
another = print_msg("Hello")
another()

# Output: Hello
```
This value in the enclosing scope is remembered even when the variable goes out of scope or the function itself is removed from the current namespace.

## When do we have closures
- We must have a nested function (function inside a function).
- The nested function must refer to a value defined in the enclosing function.
- The enclosing function must return the nested function.

## Why use closures
Closures can avoid the use of global values and provides some form of data hiding. It can also provide an object oriented solution to the problem.

When there are few methods (one method in most cases) to be implemented in a class, closures can provide an alternate and more elegant solution. But when the number of attributes and methods get larger, it's better to implement a class.

## Applications
```
def make_multiplier_of(n):
    def multiplier(x):
        return x * n
    return multiplier


# Multiplier of 3
times3 = make_multiplier_of(3)

# Multiplier of 5
times5 = make_multiplier_of(5)

# Output: 27
print(times3(9))

# Output: 15
print(times5(3))

# Output: 30
print(times5(times3(2)))
```



# Python Decorators
> https://www.programiz.com/python-programming/decorator

We must be comfortable with the fact that everything in Python (Yes! Even classes), are objects.

Functions can be passed as arguments to another function. For example, `map`, `filter`. 

Such functions that take other functions as arguments are also called **higher order functions**.

```
def inc(x):
    return x + 1


def dec(x):
    return x - 1


def operate(func, x):
    result = func(x)
    return result
```
## Examples
Basically, with the help of python closure, a decorator takes in a function, adds some functionality and returns it.

```
def make_pretty(func):
    def inner():
        print("I got decorated")
        func()
    return inner


def ordinary():
    print("I am ordinary")
```

```
>>> ordinary()
I am ordinary

>>> # let's decorate this ordinary function
>>> pretty = make_pretty(ordinary)
>>> pretty()
I got decorated
I am ordinary
```
We can see that the decorator function added some new functionality to the original function. This is similar to packing a gift. The decorator acts as a wrapper. The nature of the object that got decorated (actual gift inside) does not alter. But now, it looks pretty (since it got decorated).

## a Syntactic Sugar
```
@make_pretty
def ordinary():
    print("I am ordinary")
```
is equivalent to

```
def ordinary():
    print("I am ordinary")
ordinary = make_pretty(ordinary)
```
## Decorating Functions with Parameters
```
def smart_divide(func):
    def inner(a, b):
        print("I am going to divide", a, "and", b)
        if b == 0:
            print("Whoops! cannot divide")
            return

        return func(a, b)
    return inner


@smart_divide
def divide(a, b):
    print(a/b)
```
Without the decorator, we will get an error if b is 0. But now it won't happen.

```
>>> divide(2,5)
I am going to divide 2 and 5
0.4

>>> divide(2,0)
I am going to divide 2 and 0
Whoops! cannot divide
```
In the example above, parameters of the nested `inner()` function inside the decorator is the same as the parameters of functions it decorates. 

To make general decorators that work with any number of parameters, we can do something like this:
```
def works_for_all(func):
    def inner(*args, **kwargs):
        print("I can decorate any function")
        return func(*args, **kwargs)
    return inner
```

## Decorator with Parameters
Sometimes, the decorator itself needs some parameters. 

In this case, one more function that accepts the parameters is needed in the outer layer, to form a 3-level nested structure of functions.

```
def repeat_func(n):
    def wrapper(func):
        def inner():
            print('before function run')
            for i in range(n):
                func()
            print('after function run')
        return inner
    return wrapper

```

## Chaining Decorators
```
def star(func):
    def inner(*args, **kwargs):
        print("*" * 30)
        func(*args, **kwargs)
        print("*" * 30)
    return inner


def percent(func):
    def inner(*args, **kwargs):
        print("%" * 30)
        func(*args, **kwargs)
        print("%" * 30)
    return inner


@star
@percent
def printer(msg):
    print(msg)


printer("Hello")
```

```
******************************
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
Hello
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
******************************
```

The above syntax is equal to

```
def printer(msg):
    print(msg)
printer = star(percent(printer))
```

Here is how I understand this:

We use two layers of wrappers to decorate the gift. `star` is the outside layer and `percent` inside. So we first wrap it with `percent` only, and wrap the result we get with `star`. 



# Classmethod
> https://www.programiz.com/python-programming/methods/built-in/classmethod

A class method is a method that is bound to a class rather than its object. It doesn't require creation of a class instance.

The class method can be called both by the class and its object.

The class method is always attached to a class with the first argument as the class itself `cls`.

The syntax is:
```
@classmethod
def func(cls, args...)
```

# Property
> https://www.programiz.com/python-programming/property


```
# using property class
class Celsius:
    def __init__(self, temperature=0):
        self.temperature = temperature

    def to_fahrenheit(self):
        return (self.temperature * 1.8) + 32

    # getter
    def get_temperature(self):
        print("Getting value...")
        return self._temperature

    # setter
    def set_temperature(self, value):
        print("Setting value...")
        if value < -273.15:
            raise ValueError("Temperature below -273.15 is not possible")
        self._temperature = value

    # creating a property object
    temperature = property(get_temperature, set_temperature)


human = Celsius(37)

print(human.temperature)

print(human.to_fahrenheit())

human.temperature = -300
```
```
property(fget=None, fset=None, fdel=None, doc=None)
```

The recommended way to use property:
```
# Using @property decorator
class Celsius:
    def __init__(self, temperature=0):
        self.temperature = temperature

    def to_fahrenheit(self):
        return (self.temperature * 1.8) + 32

    @property
    def temperature(self):
        print("Getting value...")
        return self._temperature

    @temperature.setter
    def temperature(self, value):
        print("Setting value...")
        if value < -273.15:
            raise ValueError("Temperature below -273 is not possible")
        self._temperature = value


# create an object
human = Celsius(37)

print(human.temperature)

print(human.to_fahrenheit())

coldest_thing = Celsius(-300)
```
