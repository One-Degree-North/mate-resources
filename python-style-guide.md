# ODN MATE Python Style Guide
(Current Version: Oct 2021)

For Python 3.8, 3.9, 3.10+

**Disclaimer**:
Please, don't bother with this until you actually know how to program in Python. The style guide is for production code mostly by the R&D Team. Here's some resources to learn: [W3Schools](https://www.w3schools.com/python/), [learnpython](https://www.learnpython.org/), [Python Language Reference](https://docs.python.org/3/reference/index.html)

Based on [PEP 8](https://www.python.org/dev/peps/pep-0008/) and [this blog post](https://luminousmen.com/post/the-ultimate-python-style-guidelines)

### Code Layout
- Tabs:
	- Use IDE standard (prefer 4 spaces over tabs)
	- Most editors will automatically do this
- Maximum line length: either 80 characters (Old PEP 8) or 119 characters (PEP 8 Revised).
	- Just be consistent throughout the entire project. 
	- Most IDEs will default to 80 characters, please pay attention to the vertical line!
- 2 blank lines between `import`s, `class`es, and `def`s
- 1 blank line between `def`s within `class`es
- 1 blank line between different sub-algorithms (a comment too)
- No blank line between a `def` line and function contents
- No whitespace surrounding all types of brackets
	- Good: `func(arr[3], {"attr":8}, [])`
- Surround operators with whitespace on either side
	- Good: `(x + 2) == func(y % 3)`
	- Exception: default parameters
		- `def func(param=3, param_2="e"):`
	- Exception: +n and -n in algorithms
		- `arr[x+1]` `arr[x-1]`
		- `arr[x+offset]` `arr[x-offset]`
		- if you have any more complex array index calls please keep the whitespace
			- example: `arr[x + ((offset + 5) % 2)]`
- Move function arguments to match the function header if they don't fit
```py
def function_name_extended_for_clarity(param_1, param_2, 
                                       param_3, param_4):
```
- Parallelize logical conditions when the line exceeds 60 characters or if there's more than 4 conditions
```py
if (this
    and that
    or those
    and not something_else
    and extremely_long_condition_function(value_1, value_2)
   ):
```
- Chain methods logically
```py
something.set_value(val.lower()
  .strip()
  .replace("a", "b")
  .upper()
  .format('jdbc')
  .something_else()
)
					
```
- `__init__` first in a class, `__del__` last.
- use `kwargs` (keyword arguments) if possible:
	- `func(x=4, y=4, z=0.8)`
- Minimize code within a `try:` `except:` block. 
- Never use `Exception`, use something [more specific](https://docs.python.org/3/library/exceptions.html)
- No semicolons. Ever. This is python.
- Always have a `if __name__ == "__main__":` clause in a module

### Naming

```py
# variables
useful_name = "snake_case"
CONSTANT_VALUE = "UPPER_SNAKE_CASE"
__private_var = "__snake_case"
_protected_var = "_snaked_case"


# functions/methods
def snake_case_generator(snake_case_param):
	# do something


# classes
class UpperCamelCaseClass(ExtendedClass):
	"""docstring"""
	def __init__(self):
		self.i = 2
		
	def snake_case_method(self):
		self.i += 1
		return self.i
		
	def __del__(self):
		del self.i
```

Be logical with naming. While it's easy to refactor a name in an IDE it's just a hassle to clean up after you. Only include the type within a variable name if you have more than 1 variable intended to be called the same thing. (i.e. `camera_list` and `camera` (in instance))

### Formatting
- Strings
	- Use `"double quotes"` for storing data, displaying to the user, or for any strings that must be modified
	- Use `'single quotes'` for keys, constants, internal strings
	- Use `'single quotes'` for single characters
- `print()` statements
	- Production Code: `print(f"var: {var}")`
	- Debugging: `print("var:", var)`
	- Not good: `print("var: " + str(var))`
		- We don't concatenate strings unless it's absolutely necessary, since it's one of the most inefficient python built-in interfaces.
- Whole-line Comments
	- Good: `# this is what it does`
	- Bad: `#this is what it does`
- Side Comments (two spaces!)
	- Good:  `delay(1000)  // delay for 1 second`
	- Bad:  `delay(1000)//delay for 1 second`
- Static Typing
	- Whenever possible.
	- Hint: `def func(i: int, j: str) -> list:`
- Docstrings
	- Not everything needs a docstring! Leave it off if you have something better to do!
	- Indent when there's not enough space on a line
```py
class SimpleClass:
    """
	What does this class do?

	Attributes (public variables):
		name : str - the name of the instance
		id : int - the id representing the instance
	
	Methods: 
		identify(val)
			prints out its name, id, and (param) val in a formatted string.
	"""

    def identify(self, val):
        """
		prints out the class's name, id, and val.

		Parameters:
			val : any - the value to print with the class's identifier.

		Returns:
			None
		"""
        print(f"SimpleClass {name} (id: {id}) - {val}")

```
```py
# lazy way to docstring (also OK)
class SimpleClass:
	"""
	SimpleClass - class that identifies itself
	Attributes: name, id
	Methods: identify
	"""
	
	def identify(self, val):
		"identify(val) - prints out name, id, val in a formatted string"
		print(f"SimpleClass {name} (id: {id}) - {val}")
	
```
### Imports
- Avoid circular imports
- Avoid [relative imports](https://www.python.org/dev/peps/pep-0328/) if possible
- Don't use `*` in imports unless you're actually using ALL functions from it
- Imports should be written in the following order:
	- build-in modules
	- third-party modules
	- modules of the current project
```py
import os
import sys
import typing

import matplotlib.pyplot as plt
import numpy as np
from serial import Serial

from data_parser import parse_accelerometer
import acceleration
```

### Unit Testing
- Please actually do unit tests.
- Separate module unit tests from overall unit tests
	- Take advantage of the `if __name__ == "__main__":` clause for unit testing modules
	- Create separate `unit_test.py` for project overall testing
		- use Python's [`unittest`](https://docs.python.org/3/library/unittest.html) module in separate unit testing scripts
- Keep in mind a failed `assert` results in `AssertionError`
```py
def unit_test_func_a():
	result = func_a()
	# return should be a list
	assert isinstance(result, list)
	# check that func_a()[x] = func_a()[x+1] * 3
	assert result[0] == result[1] * 3
	assert result[4] == result[5] * 3
	# check that the returned list is shorter than 8
	assert len(result) < 8


if __name__ == "__main__":
	unit_test_func_a()
```
### File Naming
```
project_name/
	__init__.py
	main.py
	database.py
	unit_test.py
	language_parsers/
		__init__.py
		english_parser.py
		french_parser.py
	database_helpers/
		__init__.py
		mysql.py
		postgresql.py
```
It's all `snake_case`!

### Good Practices
- Make checks for user interface return validity
- Make checks for unreliable code (by new programmers/poorly written libraries)
- Do unit testing. Please do.
- Use `struct` for type conversion in systems programming
- Use `typing` and standard static-typing practices
- Use constants to replace "magic numbers"

### Bad Practices
- using the `global` keyword
- using `:=` when not in Python 3.9 or 3.10
- inefficient algorithms (iteration when vector operations exist)
- lambdas unless really needed
- nested maps/lambdas
- overly nested functions (more than 4 layers w/o recursion)
- dynamically opening/closing threads unless really required
