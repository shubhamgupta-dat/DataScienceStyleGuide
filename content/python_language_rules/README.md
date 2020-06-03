# 2. Python Language Rules:

It is important what you are conveying, and it is *more* important what the second person is percieving. It is important that we acknowledge the rules of language so we can communicate better. For general communication language we use grammer to make our sentences and thoughts more structured. Developers work day in and day out on the same language. Hence, for reading and writing any development language some important rules are required to be followed. 

### 2.1 Defining Imports
- Use `import x` for importing packages and modules.
- Use `from x import y where x` is the package prefix and `y` is the module name with no prefix.
- Use `from x import y as z` if two modules named `y` are to be imported or if `y` is an inconveniently long name.
- Use `import y as z` only when `z` is a standard abbreviation (e.g., `np` for `numpy` OR `pd` for `pandas`).
- Import all modules/packages in same cell and preferably at the starting of the notebook or starting of the python file.
- Group similar kind of imports in consecutive lines and separate these groups by a line space.
- Import only important modules in production level code (e.g., to use generate random values, import `numpy.random` module rather than the complete `numpy` package).
- All new code should import each module by its full package name.
  eg: Both of the below mentioned piece of codes are correct
  ```python
  # Reference absl.flags in code with the complete name (verbose).
  import absl.flags
  from doctor.who import jodie

  FLAGS = absl.flags.FLAGS
  ```
  ```python
  # Reference flags in code with just the module name (common).
  from absl import flags
  from doctor.who import jodie

  FLAGS = flags.FLAGS
  ```

  No:
  ```python
  # Unclear what module the author wanted and what will be imported.  The actual
  # import behaviour depends on external factors controlling sys.path.
  # Which possible jodie module did the author intend to import?
  import jodie
  ```
  The directory the main binary is located in should not be assumed to be in `sys.path` despite that happening in some environments. This being the case, code should assume that `import jodie` refers to a third party or top level package named `jodie`, not a local `jodie.py`.

#### Word of Caution:
- Module names can still collide. Some module names are inconveniently long. Please refrain using standard library abbreviations and keep them recognisable.
- On the same note, do not use relative names in imports. Even if the module is in the same package, use the full package name. This helps prevent unintentionally importing a package twice.

### 2.2 Catching Exceptions
- Exceptions should be used where ever possible only if used carefully. They shift the normal flow of control of a code block to handle errors and exceptional conditions.
- Raise exceptions like this: `raise MyError('Error message')` or `raise MyError()`. Do not use the two-argument form `(raise MyError, 'Error message')`
- Make use of built-in exception classes when it makes sense. For example, raise a `ValueError` to indicate a programming mistake like a violated precondition (such as if you were passed a negative number but required a positive one). Do not use `assert` statements for validating argument values of a public API.
- `assert` is used to ensure internal correctness, not to enforce correct usage nor to indicate that some *unexpected event* occurred. If an exception is desired in the latter cases, use a raise statement.
- Libraries or packages may define their own exceptions. When doing so they must inherit from an existing exception class. Exception names should end in Error and should not introduce stutter (`foo.FooError`). This fundamental can also be used in #LibraryDevelopment.
- Never use catch-all `except:` statements, or catch `Exception` or `StandardError`, unless you are
  1. re-raising the exception, or
  2. creating an isolation point in the program where exceptions are not propagated but are recorded and suppressed instead.

  While working on exploratory notebooks or test notebooks, this use can be exceptional. But it is important to avoid it on production level code.
- Minimize the amount of code in a `try`/`except` block. The larger the body of the try, the more likely that an exception will be raised by a line of code that you didn’t expect to raise an exception. In those cases, the `try`/`except` block hides a real error.
- Use the `finally` clause to execute code whether or not an exception is raised in the try block. This is often useful for cleanup, i.e., closing a file.
- When capturing an exception, use as rather than a comma. For example:
  ```python
  try:
    raise Error()
  except Error as error:
    pass
  ```

### 2.3 Global variables
- Variables that are declared at the module level or as class attributes.
- They are occasionally useful but should be avoided as much as possible.
- Constants must be named using all caps and separated by underscores.
- While they are technically variables, module-level constants are permitted and encouraged. For example: `MIN_PORT_VALUE = 1025`
- If needed, globals should be declared at the module level and made internal to the module by prepending an `_` to the name. This makes the variable protected and easy to used by the module.
- Declaring variables in Python Notebooks should be done in the earlier cells of notebooks and if required should be grouped and separated by an extra line.
- Variable declaration either should be done in separate modules or should be done in context with similar modules
- Do not change values and context of global variables in any code block.

### 2.4 Nested/Local/Inner Classes and Functions
- A class can be defined inside of a method, function, or class. A function can be defined inside a method or function. Nested functions have read-only access to variables defined in enclosing scopes.
- Allows definition of utility classes and functions that are only used inside of a very limited scope. Very ADT-y. Commonly used for implementing decorators.
- Nesting can make your outer function longer and less readable. Test Coverage also decreases if more and more nesting functions are used. Hence, it is advised to use not more than 2 nested functions or 1 nested class with bare minimum functionality.
- Avoid nested functions or classes except when closing over a local value. This will ensure the usability and reliability of the nested functions, and the state of input variable(s) reliable.
- Naming conventions like adding prefix `_` can make the method/function protected from users of module. This will ensure the usability and test coverage of functions.
- Norms are relaxed for notebooks but developers should be careful while writing nested classes and functions for code readability.

### 2.5 Comprehensions & Generator Expressions
- List, Dict, and Set comprehensions as well as generator expressions provide a concise and efficient way to create container types and iterators without resorting to the use of traditional loops, `map()`, `filter()`, or `lambda`.
- Simple comprehensions can be clearer and simpler than other dict, list, or set creation techniques. Generator expressions can be very efficient, since they avoid the creation of a list entirely.
- Multiple `for` clauses or filter expressions are strictly not permitted.
- Complicated comprehensions or generator expressions can be hard to read. And hence should be avoided when things get more complicated.

#### YES ✅
```python
result = [mapping_expr for value in iterable if filter_expr]

result = [{'key': value} for value in iterable
          if a_long_filter_expression(value)]

result = [complicated_transform(x)
          for x in iterable if predicate(x)]

descriptive_name = [
    transform({'key': key, 'value': value}, color='black')
    for key, value in generate_iterable(some_input)
    if complicated_condition_is_met(key, value)
]

result = []
for x in range(10):
    for y in range(5):
        if x * y > 10:
            result.append((x, y))

return {x: complicated_transform(x)
        for x in long_generator_function(parameter)
        if x is not None}

squares_generator = (x**2 for x in range(10))

unique_names = {user.name for user in users if user is not None}

eat(jelly_bean for jelly_bean in jelly_beans
    if jelly_bean.color == 'black')
```

#### NO ⛔️
```python
result = [complicated_transform(
              x, some_argument=x+1)
          for x in iterable if predicate(x)]

## No Multiple Nested Loops are allowed
result = [(x, y) for x in range(10) for y in range(5) if x * y > 10]

return ((x, y, z)
        for x in range(5)
        for y in range(5)
        if x != y
        for z in range(5)
        if y != z)
```

### 2.6 Default Iterators and Operators
- Container types, like dictionaries and lists, define default iterators and membership test operators (“in” and “not in”).
- The default iterators and operators are simple and efficient. They express the operation directly, without extra method calls.
- Iterable Objects/Classes have different results. Developer should be aware about the iterables. e.g. `pandas.DataFrame` is iterated on column names and not on row index but `pandas.Series` is iterated on row index.
- You can’t tell the type of objects by reading the method names (e.g. has_key() means a dictionary). This is also an advantage. Use Cautiously!
- Word of Caution: Do not mutate a container while iterating over it

#### YES ✅
```python
      for key in adict: ...
      if key not in adict: ...
      if obj in alist: ...
      for line in afile: ...
      for k, v in adict.items(): ...
      for k, v in six.iteritems(adict): ...
```

#### NO ⛔️
```python
      for key in adict.keys(): ...
      if not adict.has_key(key): ...
      for line in afile.readlines(): ...
      for k, v in dict.iteritems(): ...
```

### 2.7 Generators
- A generator function returns an iterator that yields a value each time it executes a yield statement. After it yields a value, the runtime state of the generator function is suspended until the next value is needed.
- It is suggested to use a generator, as it uses less memory and because of the state preservation for local variables and control flow. `range` is the best example of a generator.
- The code is simple and uses `yield` instead of `return`. Hence, use “Yields:” rather than “Returns:” in the docstring for generator functions.

### 2.8 Lambda Functions
- Lambdas define anonymous functions in an expression, as opposed to a statement. They are often used to define callbacks or operators for higher-order functions like `map()` and `filter()`.
- They are good for one liner functions. As a rule of thumb, if the length of lambda function exceeds 60-80 chars, it is better to define a nested function.
- Lambda functions are harder to read and debug than local functions. The lack of names means the stack traces are more difficult to understand.
- If lambda function is not very simple and is being repeated again and again its better to have a nested function. This should be applicable for both python modules and notebooks.

### 2.9 Conditional Expressions
- Conditional expressions (sometimes called a “ternary operator”) are mechanisms that provide a shorter syntax for if statements. For example: `x = 1 if cond else 2`.
- These expressions are okay for simple cases. Each portion must fit on one line:
  - true-expression
  - if-expression
  - else-expression
- Use a complete if statement when things get more complicated.

#### YES ✅
```python
one_line = 'yes' if predicate(value) else 'no'
slightly_split = ('yes' if predicate(value)
                  else 'no, nein, nyet')
the_longest_ternary_style_that_can_be_done = (
    'yes, true, affirmative, confirmed, correct'
    if predicate(value)
    else 'no, false, negative, nay')
```

#### NO ⛔️
```python
bad_line_breaking = ('yes' if predicate(value) else
                     'no')
portion_too_long = ('yes'
                    if some_long_module.some_long_predicate_function(
                        really_long_variable_name)
                    else 'no, false, negative, nay')
```

### 2.10 Default Argument Values
- Only one precaution need to be taken. Do not use mutable objects as default values in the function or method definition.
- This should be applicable for both python modules and notebooks.

#### YES ✅
```python
def foo(a, b=None):
         if b is None:
             b = []
def foo(a, b: Optional[Sequence] = None):
         if b is None:
             b = []
def foo(a, b: Sequence = ()):  # Empty tuple OK since tuples are immutable
         ...
```

#### NO ⛔️
```python
def foo(a, b=[]):
         ...
def foo(a, b=time.time()):  # The time the module was loaded???
         ...
def foo(a, b=FLAGS.my_thing):  # sys.argv has not yet been parsed...
         ...
def foo(a, b: Mapping = {}):  # Could still get passed to unchecked code
         ...
```

### 2.11 Properties
- Use properties for accessing or setting data where you would normally have used simple, lightweight accessor or setter methods.
- A way to wrap method calls for getting and setting an attribute as a standard attribute access when the computation is lightweight.
- Readability is increased by eliminating explicit get and set method calls for simple attribute access. Allows calculations to be lazy. Considered the Pythonic way to maintain the interface of a class.
- Properties should be created with the `@property` decorator.
- ⚠️ Word of Caution: If you are using Python 2.x, please inherit class from `object`.
- ⚠️ Word of Caution: Inheritance with properties can be non-obvious if the property itself is not overridden. Thus one must make sure that accessor methods are called indirectly to ensure methods overridden in subclasses are called by the property

#### YES ✅
```python
     import math

     class Square(object):
         """A square with two properties: a writable area and a read-only perimeter.

         To use:
         >>> sq = Square(3)
         >>> sq.area
         9
         >>> sq.perimeter
         12
         >>> sq.area = 16
         >>> sq.side
         4
         >>> sq.perimeter
         16
         """

         def __init__(self, side):
             self.side = side

         @property
         def area(self):
             """Area of the square."""
             return self._get_area()

         @area.setter
         def area(self, area):
             return self._set_area(area)

         def _get_area(self):
             """Indirect accessor to calculate the 'area' property."""
             return self.side ** 2

         def _set_area(self, area):
             """Indirect setter to set the 'area' property."""
             self.side = math.sqrt(area)

         @property
         def perimeter(self):
             return self.side * 4
```

### 2.12 True/False Evaluations
- ✅ Use the “implicit” false if possible, e.g., `if foo:` rather than `if foo != []:`
- Always use `if foo is None`: (or `is not None`) to check for a `None` value-e.g., when testing whether a variable or argument that defaults to None was set to some other value. The other value might be a value that’s false in a boolean context!
- Never compare a boolean variable to `False` using `==`. Use `if not x:` instead. If you need to distinguish `False` from `None` then chain the expressions, such as `if not x and x is not None:`.
- For sequences (strings, lists, tuples), use the fact that empty sequences are false, so `if seq:` and `if not seq:` are preferable to `if len(seq):` and `if not len(seq):` respectively.
- ⚠️ Word of Caution: When handling integers, implicit false may involve more risk than benefit (i.e., accidentally handling `None` as `0`). You may compare a value which is known to be an integer (and is not the result of `len()`) against the integer `0`.
- ⚠️ Word of Caution: '`0`' (i.e., `0` as string) evaluates to `true`.

#### YES ✅
```python
if not users:
  print('no users')

if foo == 0:
  self.handle_zero()

if i % 10 == 0:
  self.handle_multiple_of_ten()

def f(x=None):
  if x is None:
    x = []
```

#### NO ⛔️
```python
if len(users) == 0:
    print('no users')

if foo is not None and not foo:
    self.handle_zero()

if not i % 10:
    self.handle_multiple_of_ten()

def f(x=None):
    x = x or []
```

### 2.13 Deprecated Language Features
- Use string methods instead of the `string` module where possible.
- Use function call syntax instead of `apply`.
- Use list comprehensions and `for` loops instead of `filter` and `map` when the function argument would have been an inlined lambda anyway. This reduces the complexity.
- Use `for` loops instead of `reduce`.

#### YES ✅
```python
     words = foo.split(':')

     [x[1] for x in my_list if x[2] == 5]

     map(math.sqrt, data)    # Ok. No inlined lambda expression.

     fn(*args, **kwargs)
```

#### NO ⛔️
```python
     words = string.split(foo, ':')

     map(lambda x: x[1], filter(lambda x: x[2] == 5, my_list))

     apply(fn, args, kwargs)
```

### 2.14 Lexical Scoping
- Okay to use. Not an exceptional feature.
- A nested Python function can refer to variables defined in enclosing functions, but can not assign to them. Variable bindings are resolved using lexical scoping, that is, based on the static program text.
- ⚠️ Word of Caution: Scope of Variables should be used with caution. This has lead to a known bug: PEP-0227.

```python
def get_adder(summand1):
    """Returns a function that adds numbers to a given number."""
    def adder(summand2):
        return summand1 + summand2

    return adder
```

### 2.15 Function and Method Decorators
- A function returning another function, usually applied as a function transformation using the `@wrapper` syntax. Common examples for decorators are `classmethod()`, `staticmethod()` and `property`.
- Elegantly specifies some transformation on a method; the transformation might eliminate some repetitive code. These wrappers can be used in multiple packages/modules to increase readability and performance.
- Decorators should follow the same import and naming guidelines as functions.
- Decorator pydoc should clearly state that the function is a decorator. Write unit tests for decorators.
- ⚠️ Word of Caution: Avoid external dependencies in the decorator itself (e.g. don’t rely on files, sockets, database connections, etc.), since they might not be available when the decorator runs (at import time, perhaps from `pydoc` or other tools).
- A decorator that is called with valid parameters should (as much as possible) be guaranteed to succeed in all cases.
- ⚠️ Word of Caution: Never use `@staticmethod` unless forced to in order to integrate with an API defined in an existing library. Write a module level function instead.
- ⚠️ Word of Caution: Use `@classmethod` only when writing a named constructor or a class-specific routine that modifies necessary global state such as a process-wide cache.



### 2.16 Threading
- *DO NOT* rely on the atomicity of built-in types.
- Use `Queue` for dependencies on data and Threading.
- For threading, use functional paradigm of programming to avoid State Overlap, Concurrency and Racing issues.

### 2.17 Power Features
- Try to *avoid* power features like custom metaclasses, access to bytecode, on-the-fly compilation, dynamic inheritance, object reparenting, import hacks, reflection (e.g. some uses of getattr()), modification of system internals, etc. It's harder to read, understand and debug code.
- Use Standard Library Modules and Classes that internally use these features like, `abc.ABCMeta`, `collections.namedtuple`, `dataclasses`, and `enum`

### 2.18 Type Annotated Code
- Type annotations (or “type hints”) are for function or method arguments and return values:
```python
def func(a: int) -> List[int]:
```
- You can also declare the type of a variable using a special comment:
```python
a = SomeFunc()  # type: SomeType
```
- Though Type Annotations improve the readability and maintainability of the code, you will have to keep the type declarations up to date. You might see type errors that you think are valid code.
- It is strongly advised to enable Python type analysis, (PEP-484)[https://www.python.org/dev/peps/pep-0484/], when updating the code. Tools like mypy and pytype can help.
- Try to keep checks for important return functions. This will encourage the use of SOLID Design Principle.
