# Data Science Style Guide

> _With great power there must also come great responsibility!_

## 1. Background

Python is the primary language being used, by Data Science India Team, to develop and deploy solutions. This style guide is a list of _DOs_ and _DON'Ts_ for the team's developer. This can be referred by internal and other teams and suggestions are welcome. This style guide is inspired by the Google's style guide for python. Author(s) have added more information and modified few important points to accommodate specific use-cases for development in all kinds of environment.

These best practices can be used while working on interactive python notebooks or while writing production level code on any IDE.

### Preferred IDEs:
 - PyCharm Community Edition
 - pyzo
 - Vim

We have tried to cover important topics such as:
2. Python Language Rules
3. Python Style Rules
4. Python Usability Best Practices
5. Jupyter Notebook Best Practices  
6. Library Development Best Practices  

## 2. Python Language Rules:

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
  #### YES ✅
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

  #### NO ⛔️
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






--------------
## 🚧 Men at Work! 🚧

## Coming Soon, Near You!

- Python Style Rules
- Python Usability Best Practices
- Jupyter Notebook Best Practices
- Library Development Best Practices
