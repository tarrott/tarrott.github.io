# Python

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
---
## Package Management

### pip
- pip freeze > requirements.txt
- pip install requirements.txt

### pipenv