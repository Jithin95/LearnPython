##### Assert Function

check if foo method is defined in base class in compile time itself.
> library.py

```python
class Base:
    def foo(self):
        return "Foo function"
```

> user.py

```python
from library import Base
assert hasattr(Base, "foo"), "Assertion catched"
class Derived(Base):
    def bar(self):
        self.foo()

```
##### Check whether a function is defined on Derived class from Base class

> bar() should be defined on derived class

```python
class Base:
    def foo(self):
        return self.bar()
```

```python
from library import Base
class Derived(Base):
    def bar(self):
        print ("Derived: bar function")

Derived().foo()
```

> solution 1 using build_class

```python
class Base:
    def foo(self):
        return self.bar()

old_bc = __build_class__
def my_bc(fun, name, base= None, **kw):
    if base is Base:
        print ("Check if bar is method is defined")

    if base is not None:
        return old_bc(fun, name, base, **kw)

    return old_bc(fun, name, **kw)


import builtins
builtins.__build_class__ = my_bc
```

> solution 2 using meta class

```python
class BaseMeta(type):
    def __new__(cls, name, base, body):
        if name == 'Derived' and not 'bar' in body:
            raise TypeError("Bad User class")
        print ("Basemeta __new__ => " , cls, name, base, body)
        return super().__new__(cls, name, base, body)

class Base(metaclass = BaseMeta):
    def foo(self):
        return self.bar()
```

> solution 3 wit  init_subclass

```python
class Base():
    def foo(self):
        return self.bar()

    def __init_subclass__(cls, **kwarg):
        assert hasattr(cls, "bar")
        return super.__init_subclass__(**kwarg)

```

## Decorators
```python
from time import time

def timer(func):
    def f(x, y=10): # def f(*args, **kwargs) Generalisation of function
        before = time()
        rv = func(x, y) # func(*args, **kwargs)
        after = time()
        print ("Elapsed time ", after - before)
        return rv
    return f

@timer
def add(x, y=10):
    return x+y

@timer
def sub(x, y=10):
    return x-y
# addFunc = timer(add)
# @timer which is equivalent to addFunc = timer(add)
#addFunc(add(20))
print ("Add 20 => ", add(20))
#addFunc(add(20, 30))
print ("Add 20,30 => ", add(20,30))
#addFunc(sub(20))
print ("Sub 20 => ", sub(20))
#addFunc(sub(20, 30))
print ("Sub 20,30 => ", sub(20,30))
```
> Running a function n times using Decorators

```python
from time import time

def timer(func):
    def f(*args, **kwargs):
        before = time()
        rv = func(*args, **kwargs)
        after = time()
        print ("Elapsed time ", after - before)
        return rv
    return f


def ntimes(n):
    def inner(f):
        def wrapper(*args, **kwargs):
            for _ in range(n):
                print ("Running {.__name__}".format(f))
                rv = f(*args, **kwargs)
            return rv
        return wrapper
    return inner

@ntimes(5)
def add(x, y=10):
    return x+y

print ("Add 30 , 40" , add(30, 40))
```

## Generators

```python
class GenClass:
    def __iter__(self):
        print ("__iter__", __name__)
        self.last = 10
        return self

    def __next__(self):
        rv = self.last
        self.last += 1
        if self.last > 20:
            raise StopIteration
        sleep(.5)
        return rv

```


> using Generators

```python
def GenFunc():
    for i in range(10):
        sleep(.5)
        yield i

for i in GenFunc():
    print (i)

```
