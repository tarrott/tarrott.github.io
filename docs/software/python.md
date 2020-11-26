# Python

## Pyenv
`brew install pyenv`
`brew install pyenv-virtualenv`
install version: `pyenv instal [version]`
set version: `pyenv global [version]`
list installed versions: `pyenv versions`
list virtualenvs: `pyenv virtualenvs`
activate virutalenv: `pyenv activate`

#### Update .bashrc
```
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
export PATH="$HOME/.pyenv/bin:$PATH"
```

## Enumerate Iterable
`enumerate(iterable, start=0)`
```
for index, element in enumerate(l1,100): 
    print index, element 
```

---
## Higher-order functions
From Composing Programs by John DeNero, Chapter 1.6.1:  
```
def summation(n, term):
    total, k = 0, 1
    while k <= n:
        total, k = total + term(k), k + 1
    return total

def cube(x):
    return x*x*x

def sum_cubes(n):
    return summation(n, cube)

result = sum_cubes(3)
```


### Nest Definitions and Lexical Scope
From Composing Programs by John DeNero, Chapter 1.6.3:  
```
def average(x, y):
    return (x + y)/2

def improve(update, close, guess=1):
    while not close(guess):
        guess = update(guess)
    return guess

def approx_eq(x, y, tolerance=1e-3):
    return abs(x - y) < tolerance

def sqrt(a):
    def sqrt_update(x):
        return average(x, a/x)
    def sqrt_close(x):
        return approx_eq(x * x, a)
    return improve(sqrt_update, sqrt_close)

result = sqrt(256)
```
Notice how the nested definition `sqrt_update` still has access to the `a`, which is a formal parameter of its enclosing function `sqrt`. The nested definition extends its parent environment.

"The sqrt_update function carries with it some data: the value for a referenced in the environment in which it was defined. Because they "enclose" information in this way, locally defined functions are often called `closures`."

### Decorators
import time
from functools import wraps 
def timethis(func): 
''' Decorator that reports the execution time. ''' 
        @wraps(func)
        def wrapper(*args, **kwargs): 

        start = time.time()
        result = func(*args, **kwargs) end = time.time() 
        print(func.__name__, end-start) 
        return result 
    return wrapper

@timethis
def countdown(n):
'''Counts down'''
    whilen>0:
        n -= 1

> countdown(100000) 
countdown 0.008917808532714844
> countdown(10000000) 
countdown 0.87188299392912

---
## Python Requests Lib

#### Timeout
- timeout request after 30 seconds: `resquests.get('https://example.org', timeout=30)`

#### Handle Errors
- base-class exception: `except requests.exceptions.RequestException as e: raise SystemExit(e)`
- catch each exception separately: `except requests.exceptions.Timeout`, `TooManyRedirects`, or `RequestException`
- raise exception for HTTP errors: try `Response.raise_for_status()` and `except requests.exceptions.HTTPError as err`

---
## Type Checking
- type checker module: `pip install mypy`
- examples:
    - method signature: `def add(a: int, b: int) -> int:`
    - variable declaration: `agw: int = 99`

---
## Style Guidlines: PEP8 & Linters
- [PEP8](https://pep8.org)
- PEP20: Python Zen
- PEP287: Doc-String
- PEP463: Exceptions-catching
- PEP484: Type Hints

### Formatting
- Docstrings for all public modules, functions, classes and methods
- imports separated in 3 groups by blank lines
    - standard library
    - 3rd party libraries
    - local application/library

### Whitespace/punctuation
- identations: 4 spaces per lect
- max line length: 79 characters
- 2 lines between top-level functions and classes
- 1 line between methods inside a class

### Naming
- modules: short, lowercase names
- classes: CapitalizedNaming
- functions: lowercase_with_underscores
- constants: ALL_CAPS
- non-public names: _start_with_underscore

### Linters
#### pylint
- checks for PEP8 and other code smells
- `pip install pylint`
- `pylint <package>` or `pylint <package>/file.py`
- add pylintrc file: `pylint --generate-rcfile > pylintrc`

#### Black
- great for CI pipelines to enforce consistency in source code
- `pip install black`
- `black <package>`

---
## Generating Documentation
- PEP 257: semantics and conventions for docstrings

### Docstrings
- string as first statement statements of a module, function, class or method
- becomes the `__doc__` atribute, callable in python
- always use `"""three double quotes"""`
- end phrases in a period
- methods should specify a return value

### Sphinx
- generates HTML, PDF, eBook, Linux man pages, etc. from source
- extracts docstrings from code
- accepts reStructured Text to build documentation pages
- generate API docs: `sphinx-apidoc -o <package>`

#### Quickstart
1. `pip install sphinx`
2. `mkdir docs && cd docs`
3. `sphinx-quickstart`
4. `make html`
5. clean old builds: `make clean html`

Tip: Use reStructured Text in Docstrings to automatically generate documentation:
module Docstring:
```
"""
    module.py
    ---------

    This module contains a module-level docstring
"""

class MyClass:
    """
    The MyClass class represents a generic class. Link to the :meth:`run` method.
    """

    def run(self, name):
        """
        Runs the class with the name of :attr:`name`.

        :param name : The name of the class
        :return: The new value of :attr:`name`.
        """
```
---
## Package Management

### pip
- pip freeze > requirements.txt
- pip install requirements.txt

### pipenv

---
## Common Errors
- RescursionError = stack overflow


---
## Sets
```
light_bulbs = set()

light_bulbs.add('incandescent')
light_bulbs.add('compact fluorescent')
light_bulbs.add('LED')

'LED' in light_bulbs  # True
'halogen' in light_bulbs  # False
```

---
## VSCode

#### Set Syntax
Command Pallete > languages  
"Change Language Mode"  
e.g. "SQL Formatter" Extension

#### Format File in Syntax
Shift + Option + F (Mac)

