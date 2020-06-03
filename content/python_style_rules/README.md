# 3 Python Style Rules 

#TODO: Add Description

### 3.1 Semicolons 

Do not terminate your lines with semicolons, and do not use semicolons to put
two statements on the same line.

### 3.2 Line length 

- Maximum line length is *80 characters*.

- Explicit exceptions to the 80 character limit:

  -   Long import statements.
  -   URLs, pathnames, or long flags in comments.
  -   Long string module level constants not containing whitespace that would be inconvenient to split across lines such as URLs or pathnames.
  -   Pylint disable comments. (e.g.: `# pylint: disable=invalid-name`)

- Do not use backslash line continuation except for `with` statements requiring three or more context managers.

- Make use of Python's [implicit line joining inside parentheses, brackets and braces](http://docs.python.org/reference/lexical_analysis.html#implicit-line-joining). If necessary, you can add an extra pair of parentheses around an expression.

```python
 ✅: foo_bar(self, width, height, color='black', design=None, x='foo',
             emphasis=None, highlight=0)

     if (width == 0 and height == 0 and
         color == 'red' and emphasis == 'strong'):
```

- When a literal string won't fit on a single line, use parentheses for implicit line joining.

```python
x = ('This will build a very long long '
     'long long long long long long string')
```

- Within comments, put long URLs on their own line if necessary.

```python
 ✅:  # See details at
      # http://www.example.com/us/developer/documentation/api/content/v2.0/csv_file_name_extension_full_specification.html
```

```python
⛔️:  # See details at
     # http://www.example.com/us/developer/documentation/api/content/\
     # v2.0/csv_file_name_extension_full_specification.html
```

- It is permissible to use backslash continuation when defining a `with` statement whose expressions span three or more lines. For two lines of expressions, use  nested `with` statement:

```python
 ✅:  with very_long_first_expression_function() as spam, \
           very_long_second_expression_function() as beans, \
           third_thing() as eggs:
          place_order(eggs, beans, spam, beans)
```

```python
⛔️:  with VeryLongFirstExpressionFunction() as spam, \
          VeryLongSecondExpressionFunction() as beans:
       PlaceOrder(eggs, beans, spam, beans)
```

```python
 ✅:  with very_long_first_expression_function() as spam:
          with very_long_second_expression_function() as beans:
              place_order(beans, spam)
```

### 3.3 Parentheses 

- Use parentheses sparingly.
- It is fine, though not required, to use parentheses around tuples. Do not use them in return statements or conditional statements unless using parentheses for implied line continuation or to indicate a tuple.

```python
 ✅: if foo:
         bar()
     while x:
         x = bar()
     if x and y:
         bar()
     if not x:
         bar()
     # For a 1 item tuple the ()s are more visually obvious than the comma.
     onesie = (foo,)
     return foo
     return spam, beans
     return (spam, beans)
     for (x, y) in dict.items(): ...
```

```python
⛔️:  if (x):
         bar()
     if not(x):
         bar()
     return (foo)
```

### 3.4 Indentation 

- Indent your code blocks with *4 spaces*.

- Never use tabs or mix tabs and spaces. In cases of implied line continuation, you should align wrapped elements either vertically, as per the examples in the line length section; or using a hanging indent of 4 spaces, in which case there should be nothing after the open parenthesis or bracket on the first line.

```python
 ✅:   # Aligned with opening delimiter
       foo = long_function_name(var_one, var_two,
                                var_three, var_four)
       meal = (spam,
               beans)

       # Aligned with opening delimiter in a dictionary
       foo = {
           long_dictionary_key: value1 +
                                value2,
           ...
       }

       # 4-space hanging indent; nothing on first line
       foo = long_function_name(
           var_one, var_two, var_three,
           var_four)
       meal = (
           spam,
           beans)

       # 4-space hanging indent in a dictionary
       foo = {
           long_dictionary_key:
               long_dictionary_value,
           ...
       }
```

```python
⛔️:    # Stuff on first line forbidden
       foo = long_function_name(var_one, var_two,
           var_three, var_four)
       meal = (spam,
           beans)

       # 2-space hanging indent forbidden
       foo = long_function_name(
         var_one, var_two, var_three,
         var_four)

       # No hanging indent in a dictionary
       foo = {
           long_dictionary_key:
           long_dictionary_value,
           ...
       }
```


### 3.4.1 Trailing commas in sequences of items? 

Trailing commas in sequences of items are recommended only when the closing
container token `]`, `)`, or `}` does not appear on the same line as the final element.

```python
 ✅:   golomb3 = [0, 1, 3]
 ✅:   golomb4 = [
           0,
           1,
           4,
           6,
       ]
```

```python
⛔️:    golomb4 = [
           0,
           1,
           4,
           6
       ]
```

<a id="s3.5-blank-lines"></a>
<a id="35-blank-lines"></a>

<a id="blank-lines"></a>
### 3.5 Blank Lines 

- *Two blank* lines between top-level definitions, be they function or class
definitions. 
- *One blank* line between method definitions and between the `class`
line and the first method. 
- *No blank* line following a `def` line. Use single
blank lines as you judge appropriate within functions or methods.

<a id="s3.6-whitespace"></a>
<a id="36-whitespace"></a>

<a id="whitespace"></a>
### 3.6 Whitespace 

- Follow standard typographic rules for the use of spaces around punctuation.

- No whitespace inside parentheses, brackets or braces.

```python
 ✅: spam(ham[1], {eggs: 2}, [])
```

```python
⛔️:  spam( ham[ 1 ], { eggs: 2 }, [ ] )
```

- No whitespace before a comma, semicolon, or colon. Do use whitespace after a comma, semicolon, or colon, except at the end of the line.

```python
 ✅: if x == 4:
         print(x, y)
     x, y = y, x
```

```python
⛔️:  if x == 4 :
         print(x , y)
     x , y = y , x
```

- No whitespace before the open paren/bracket that starts an argument list, indexing or slicing.

```python
 ✅: spam(1)
```

```python
⛔️:  spam (1)
```

- No trailing whitespace.

```python
 ✅: dict['key'] = list[index]
```

```python
⛔️:  dict ['key'] = list [index]
```
- Surround binary operators with a single space on either side for assignment (`=`), comparisons (`==, <, >, !=, <>, <=, >=, in, not in, is, is not`), and Booleans (`and, or, not`). Use your better judgment for the insertion of space around arithmetic operators (`+`, `-`, `*`, `/`, `//`, `%`, `**`, `@`).

```python
 ✅: x == 1
```

```python
⛔️:  x<1
```

- Never use spaces around `=` when passing keyword arguments or defining a default parameter value, with one exception: [when a type annotation is present](#typing-default-values), _do_ use spaces around the `=` for the default parameter value.

```python
 ✅: def complex(real, imag=0.0): return Magic(r=real, i=imag)
 ✅: def complex(real, imag: float = 0.0): return Magic(r=real, i=imag)
```

```python
⛔️:  def complex(real, imag = 0.0): return Magic(r = real, i = imag)
⛔️:  def complex(real, imag: float=0.0): return Magic(r = real, i = imag)
```

- Don't use spaces to vertically align tokens on consecutive lines, since it becomes a maintenance burden (applies to `:`, `#`, `=`, etc.):

```python
 ✅:
  foo = 1000  # comment
  long_name = 2  # comment that should not be aligned

  dictionary = {
      'foo': 1,
      'long_name': 2,
  }
```

```python
⛔️:
  foo       = 1000  # comment
  long_name = 2     # comment that should not be aligned

  dictionary = {
      'foo'      : 1,
      'long_name': 2,
  }
```


<a id="Python_Interpreter"></a>
<a id="s3.7-shebang-line"></a>
<a id="37-shebang-line"></a>

<a id="shebang-line"></a>

### 3.7 Comments and Docstrings 

Be sure to use the right style for module, function, method docstrings and
inline comments.

<a id="s3.7.1-comments-in-doc-strings"></a>
<a id="371-docstrings"></a>
<a id="comments-in-doc-strings"></a>

<a id="docstrings"></a>
#### 3.7.1 Docstrings 

- Python uses _docstrings_ to document code. A docstring is a string that is the first statement in a package, module, class or function. These strings can be extracted automatically through the `__doc__` member of the object and are used by `pydoc`.

- It is recommended that if complete description of package, module, class or function cannot be provided docstring should be provided. 

A docstring should be organized as a summary line (one physical line) followed by a blank line, followed by the rest of the docstring starting at the same cursor position as the first quote of the first line. There are more formatting guidelines for docstrings below.

<a id="s3.7.2-comments-in-modules"></a>
<a id="382-modules"></a>
<a id="comments-in-modules"></a>

<a id="module-docs"></a>
#### 3.7.2 Modules 

- Every file should contain license boilerplate. If pushing into production. Choose the appropriate boilerplate for the license used by the project (for example, Apache 2.0, BSD, LGPL, GPL)

- Files should start with a docstring describing the contents and usage of the module.

```python
"""A one line summary of the module or program, terminated by a period.

Leave one blank line.  The rest of this docstring should contain an
overall description of the module or program.  Optionally, it may also
contain a brief description of exported classes and functions and/or usage
examples.

  Typical usage example:

  foo = ClassFoo()
  bar = foo.FunctionBar()
"""
```


<a id="s3.7.3-functions-and-methods"></a>
<a id="373-functions-and-methods"></a>
<a id="functions-and-methods"></a>

<a id="function-docs"></a>
#### 3.7.3 Functions and Methods 

In this section, "function" means a method, function, or generator.

- A function must have a docstring, unless it meets all of the following criteria:
  -   not externally visible
  -   very short
  -   obvious

- A docstring should give enough information to write a call to the function without reading the function's code. The docstring should be descriptive-style (`"""Fetches rows from a Bigtable."""`) rather than imperative-style (`"""Fetch rows from a Bigtable."""`), except for `@property` data descriptors, which should use the <a href="#374-classes">same style as attributes</a>. A docstring should describe the function's calling syntax and its semantics, not its implementation. 

- For tricky code, comments alongside the code are more
appropriate than using docstrings.

- A method that overrides a method from a base class may have a simple docstring, such as `"""See base class."""`. The rationale is that there is no need to repeat in many place documentation that is already present in the base method's docstring. However, if the overriding method's behavior is substantially different from the overridden method, or details need to be provided (e.g., documenting additional side effects), a docstring with at least those differences is required on the overriding method.

- Certain aspects of a function should be documented in special sections, listed below. Each section begins with a heading line, which ends with a colon. All sections other than the heading should maintain a hanging indent of two or four spaces (be consistent within a file). These sections can be omitted in cases where the function's name and signature are informative enough that it can be aptly described using a one-line docstring.

```python
def fetch_bigtable_rows(big_table, keys, other_silly_variable=None):
    """Fetches rows from a Bigtable.

    Retrieves rows pertaining to the given keys from the Table instance
    represented by big_table.  Silly things may happen if
    other_silly_variable is not None.

    Args:
        big_table: An open Bigtable Table instance.
        keys: A sequence of strings representing the key of each table row
            to fetch.
        other_silly_variable: Another optional variable, that has a much
            longer name than the other args, and which does nothing.

    Returns:
        A dict mapping keys to the corresponding table row data
        fetched. Each row is represented as a tuple of strings. For
        example:

        {'Serak': ('Rigel VII', 'Preparer'),
         'Zim': ('Irk', 'Invader'),
         'Lrrr': ('Omicron Persei 8', 'Emperor')}

        If a key from the keys argument is missing from the dictionary,
        then that row was not found in the table.

    Raises:
        IOError: An error occurred accessing the bigtable.Table object.
    """
```

<a id="s3.7.4-comments-in-classes"></a>
<a id="374-classes"></a>
<a id="comments-in-classes"></a>

<a id="class-docs"></a>
#### 3.7.4 Classes 

- Classes should have a docstring below the class definition describing the class.
- If your class has public attributes, they should be documented here in an
`Attributes` section and follow the same formatting as a
[function's `Args`](#doc-function-args) section.

```python
class SampleClass(object):
    """Summary of class here.

    Longer class information....
    Longer class information....

    Attributes:
        likes_spam: A boolean indicating if we like SPAM or not.
        eggs: An integer count of the eggs we have laid.
    """

    def __init__(self, likes_spam=False):
        """Inits SampleClass with blah."""
        self.likes_spam = likes_spam
        self.eggs = 0

    def public_method(self):
        """Performs operation blah."""
```

<a id="comments-in-block-and-inline"></a>
<a id="s3.7.5-comments-in-block-and-inline"></a>
<a id="375-block-and-inline-comments"></a>

<a id="comments"></a>
#### 3.7.5 Block and Inline Comments 

- The final place to have comments is in tricky parts of the code. If you're going to have to explain it at the next [code review](http://en.wikipedia.org/wiki/Code_review), you should comment it
now.

```python
# We use a weighted dictionary search to find out where i is in
# the array.  We extrapolate position based on the largest num
# in the array and the array size and then do binary search to
# get the exact number.

if i & (i-1) == 0:  # True if i is 0 or a power of 2.
```

- To improve legibility, these comments should start at least 2 spaces away from the code with the comment character `#`, followed by at least one space before the text of the comment itself.

```python
# BAD COMMENT: Now go through the b array and make sure whenever i occurs
# the next element is i+1
```

<!-- The next section is copied from the C++ style guide. -->

<a id="s3.7.6-punctuation-spelling-and-grammar"></a>
<a id="376-punctuation-spelling-and-grammar"></a>
<a id="spelling"></a>
<a id="punctuation"></a>
<a id="grammar"></a>

<a id="punctuation-spelling-grammar"></a>
#### 3.7.6 Punctuation, Spelling, and Grammar 

- Pay attention to punctuation, spelling, and grammar; it is easier to read
well-written comments than badly written ones.

- Comments should be as readable as narrative text, with proper capitalization and punctuation. In many cases, complete sentences are more readable than sentence fragments.

<a id="s3.8-classes"></a>
<a id="38-classes"></a>

<a id="classes"></a>
### 3.8 Classes 

If a class inherits from no other base classes, explicitly inherit from
`object`. This also applies to nested classes.

```python
 ✅: class SampleClass(object):
         pass


     class OuterClass(object):

         class InnerClass(object):
             pass


     class ChildClass(ParentClass):
         """Explicitly inherits from another class already."""

```

```python
⛔️: class SampleClass:
        pass


    class OuterClass:

        class InnerClass:
            pass
```

<a id="s3.9-strings"></a>
<a id="39-strings"></a>

<a id="strings"></a>
### 3.9 Strings 

- Use the `format` method or the `%` operator for formatting strings, even when
the parameters are all strings. Use your best judgment to decide between `+` and
`%` (or `format`) though.

```python
 ✅: x = a + b
     x = '%s, %s!' % (imperative, expletive)
     x = '{}, {}'.format(first, second)
     x = 'name: %s; score: %d' % (name, n)
     x = 'name: {}; score: {}'.format(name, n)
     x = f'name: {name}; score: {n}'  # Python 3.6+
```

```python
⛔️: x = '%s%s' % (a, b)  # use + in this case
    x = '{}{}'.format(a, b)  # use + in this case
    x = first + ', ' + second
    x = 'name: ' + name + '; score: ' + str(n)
```

- Avoid using the `+` and `+=` operators to accumulate a string within a loop. Since strings are immutable, this creates unnecessary temporary objects and results in quadratic rather than linear running time. Instead, add each substring to a list and `''.join` the list after the loop terminates (or, write each substring to a `io.BytesIO` buffer).

```python
 ✅: items = ['<table>']
     for last_name, first_name in employee_list:
         items.append('<tr><td>%s, %s</td></tr>' % (last_name, first_name))
     items.append('</table>')
     employee_table = ''.join(items)
```

```python
⛔️: employee_table = '<table>'
    for last_name, first_name in employee_list:
        employee_table += '<tr><td>%s, %s</td></tr>' % (last_name, first_name)
    employee_table += '</table>'
```

- Be consistent with your choice of string quote character within a file. Pick `'` or `"` and stick with it. It is okay to use the other quote character on a string to avoid the need to `\\ ` escape within the string.

```python
 ✅:
  Python('Why are you hiding your eyes?')
  Gollum("I'm scared of lint errors.")
  Narrator('"Good!" thought a happy Python reviewer.')
```

```python
⛔️:
  Python("Why are you hiding your eyes?")
  Gollum('The lint. It burns. It burns us.')
  Gollum("Always the great lint. Watching. Watching.")
```

- Prefer `"""` for multi-line strings rather than `'''`. 

- Multi-line strings do not flow with the indentation of the rest of the program. If you need to avoid embedding extra space in the string, use either concatenated single-line strings or a multi-line string with [`textwrap.dedent()`](https://docs.python.org/3/library/textwrap.html#textwrap.dedent) to remove the initial space on each line:

```python
  ⛔️:
  long_string = """This is pretty ugly.
Don't do this.
"""
```

```python
   ✅:
  long_string = """This is fine if your use case can accept
      extraneous leading spaces."""
```

```python
   ✅:
  long_string = ("And this is fine if you can not accept\n" +
                 "extraneous leading spaces.")
```

```python
   ✅:
  long_string = ("And this too is fine if you can not accept\n"
                 "extraneous leading spaces.")
```

```python
   ✅:
  import textwrap

  long_string = textwrap.dedent("""\
      This is also fine, because textwrap.dedent()
      will collapse common leading spaces in each line.""")
```

<a id="s3.10-files-and-sockets"></a>
<a id="310-files-and-sockets"></a>
<a id="files-and-sockets"></a>

<a id="files"></a>

### 3.11 Files and Sockets 

- Explicitly close files and sockets when done with them. Leaving files, sockets or other file-like objects open unnecessarily has many
downsides.

- Furthermore, while files and sockets are automatically closed when the fileobject is destructed, tying the lifetime of the file object to the state of the file is poor practice:

  -  There are no guarantees as to when the runtime will actually run the file's destructor. Different Python implementations use different memory management techniques, such as delayed Garbage Collection, which may increase the object's lifetime arbitrarily and indefinitely
  -  Unexpected references to the file, e.g. in globals or exception tracebacks, may keep it around longer than intended.

- The preferred way to manage files is using the ["with"
statement](http://docs.python.org/reference/compound_stmts.html#the-with-statement):

```python
with open("hello.txt") as hello_file:
    for line in hello_file:
        print(line)
```

- For file-like objects that do not support the "with" statement, use
`contextlib.closing()`:

```python
import contextlib

with contextlib.closing(urllib.urlopen("http://www.python.org/")) as front_page:
    for line in front_page:
        print(line)
```

<a id="s3.12-todo-comments"></a>
<a id="312-todo-comments"></a>

<a id="todo"></a>
### 3.12 TODO Comments 

- Use `TODO` comments for code that is temporary, a short-term solution, or
good-enough but not perfect.

- A `TODO` comment begins with the string `TODO` in all caps and a parenthesized name, e-mail address, or other identifier of the person or issue with the best context about the problem. This is followed by an explanation of what there is to do.

- The purpose is to have a consistent `TODO` format that can be searched to find out how to get more details. 

- A `TODO` is not a commitment that the person referenced will fix the problem. Thus when you create a `TODO`, it is almost always your name that is given.

```python
# TODO(kl@gmail.com): Use a "*" here for string repetition.
# TODO(Zeke) Change this to use relations.
```

<a id="s3.12-imports-formatting"></a>
<a id="312-imports-formatting"></a>

<a id="imports-formatting"></a>
### 3.12 Imports formatting 

Imports should be on separate lines.

E.g.:

```python
 ✅: import os
     import sys
```

```python
⛔️:  import os, sys
```

- Imports are always put at the top of the file, just after any module comments and docstrings and before module globals and constants. 
- Imports should be grouped from most generic to least generic.

1.  Python future import statements. For example:

    ```python
    from __future__ import absolute_import
    from __future__ import division
    from __future__ import print_function
    ```

2.  Python standard library imports. For example:

    ```python
    import sys
    ```

3.  [third-party](https://pypi.org/) module
    or package imports. For example:

    
    ```python
    import tensorflow as tf
    ```

4.  Code repository
    sub-package imports. For example:

    ```python
    from otherproject.ai import mind
    ```

5.  **Deprecated:** application-specific imports that are part of the same
    top level
    sub-package as this file. For example:

    
    ```python
    from myproject.backend.hgwells import time_machine
    ```

    
- Within each grouping, imports should be sorted lexicographically, ignoring case, according to each module's full package path. Code may optionally place a blank line between import sections.

```python
import collections
import queue
import sys

from absl import app
from absl import flags
import bs4
import cryptography
import tensorflow as tf

from book.genres import scifi
from myproject.backend.hgwells import time_machine
from myproject.backend.state_machine import main_loop
from otherproject.ai import body
from otherproject.ai import mind
from otherproject.ai import soul

```


<a id="s3.13-statements"></a>
<a id="313-statements"></a>

<a id="statements"></a>
### 3.13 Statements 

- Generally only one statement per line. However, you may put the result of a test on the same line as the test only if the entire statement fits on one line. 
- In particular, you can never do so with `try`/`except` since the `try` and `except` can't both fit on the same line, and you can only do so with an `if` if there is no `else`.

```python
 ✅:

  if foo: bar(foo)
```

```python
⛔️:

  if foo: bar(foo)
  else:   baz(foo)

  try:               bar(foo)
  except ValueError: baz(foo)

  try:
      bar(foo)
  except ValueError: baz(foo)
```

<a id="s3.14-access-control"></a>
<a id="314-access-control"></a>
<a id="access-control"></a>

<a id="accessors"></a>
### 3.14 Accessors 

If an accessor function would be trivial, you should use public variables
instead of accessor functions to avoid the extra cost of function calls in
Python. When more functionality is added you can use `property` to keep the
syntax consistent.


<a id="s3.15-naming"></a>
<a id="315-naming"></a>

<a id="naming"></a>
### 3.15 Naming 

`module_name`, `package_name`, `ClassName`, `method_name`, `ExceptionName`,
`function_name`, `GLOBAL_CONSTANT_NAME`, `global_var_name`, `instance_var_name`,
`function_parameter_name`, `local_var_name`.


Function names, variable names, and filenames should be descriptive; eschew
abbreviation. In particular, do not use abbreviations that are ambiguous or
unfamiliar to readers outside your project, and do not abbreviate by deleting
letters within a word.

Always use a `.py` filename extension. Never use dashes.

<a id="s3.16.1-names-to-avoid"></a>
<a id="3161-names-to-avoid"></a>

<a id="names-to-avoid"></a>
#### 3.15.1 Names to Avoid 

-   single character names except for counters or iterators. You may use "e" as an exception identifier in `try`/`except` statements.
-   dashes (`-`) in any package/module name
-   `__double_leading_and_trailing_underscore__` names (reserved by Python)

<a id="s3.15.2-naming-conventions"></a>
<a id="3152-naming-convention"></a>

<a id="naming-conventions"></a>
#### 3.15.2 Naming Conventions 

-   "Internal" means internal to a module, or protected or private within a
    class.

-   Prepending a single underscore (`_`) has some support for protecting module
    variables and functions (not included with `from module import *`). While
    prepending a double underscore (`__` aka "dunder") to an instance variable
    or method effectively makes the variable or method private to its class
    (using name mangling) we discourage its use as it impacts readability and
    testability and isn't *really* private.

-   Place related classes and top-level functions together in a
    module.
    Unlike Java, there is no need to limit yourself to one class per module.

-   Use CapWords for class names, but lower\_with\_under.py for module names.
    Although there are some old modules named CapWords.py, this is now
    discouraged because it's confusing when the module happens to be named after
    a class. ("wait -- did I write `import StringIO` or `from StringIO import
    StringIO`?")

-   Underscores may appear in *unittest* method names starting with `test` to
    separate logical components of the name, even if those components use
    CapWords. One possible pattern is `test<MethodUnderTest>_<state>`; for
    example `testPop_EmptyStack` is okay. There is no One Correct Way to name
    test methods.

<a id="s3.16.3-file-naming"></a>
<a id="3163-file-naming"></a>

<a id="file-naming"></a>
#### 3.16.3 File Naming 

Python filenames must have a `.py` extension and must not contain dashes (`-`).
This allows them to be imported and unittested. If you want an executable to be
accessible without the extension, use a symbolic link or a simple bash wrapper
containing `exec "$0.py" "$@"`.

<a id="s3.16.4-guidelines-derived-from-guidos-recommendations"></a>
<a id="3164-guidelines-derived-from-guidos-recommendations"></a>

<a id="guidelines-derived-from-guidos-recommendations"></a>
#### 3.16.4 Guidelines derived from Guido's Recommendations 

<table rules="all" border="1" summary="Guidelines from Guido's Recommendations"
       cellspacing="2" cellpadding="2">

  <tr>
    <th>Type</th>
    <th>Public</th>
    <th>Internal</th>
  </tr>

  <tr>
    <td>Packages</td>
    <td><code>lower_with_under</code></td>
    <td></td>
  </tr>

  <tr>
    <td>Modules</td>
    <td><code>lower_with_under</code></td>
    <td><code>_lower_with_under</code></td>
  </tr>

  <tr>
    <td>Classes</td>
    <td><code>CapWords</code></td>
    <td><code>_CapWords</code></td>
  </tr>

  <tr>
    <td>Exceptions</td>
    <td><code>CapWords</code></td>
    <td></td>
  </tr>

  <tr>
    <td>Functions</td>
    <td><code>lower_with_under()</code></td>
    <td><code>_lower_with_under()</code></td>
  </tr>

  <tr>
    <td>Global/Class Constants</td>
    <td><code>CAPS_WITH_UNDER</code></td>
    <td><code>_CAPS_WITH_UNDER</code></td>
  </tr>

  <tr>
    <td>Global/Class Variables</td>
    <td><code>lower_with_under</code></td>
    <td><code>_lower_with_under</code></td>
  </tr>

  <tr>
    <td>Instance Variables</td>
    <td><code>lower_with_under</code></td>
    <td><code>_lower_with_under</code> (protected)</td>
  </tr>

  <tr>
    <td>Method Names</td>
    <td><code>lower_with_under()</code></td>
    <td><code>_lower_with_under()</code> (protected)</td>
  </tr>

  <tr>
    <td>Function/Method Parameters</td>
    <td><code>lower_with_under</code></td>
    <td></td>
  </tr>

  <tr>
    <td>Local Variables</td>
    <td><code>lower_with_under</code></td>
    <td></td>
  </tr>

</table>

While Python supports making things private by using a leading double underscore
`__` (aka. "dunder") prefix on a name, this is discouraged. Prefer the use of a
single underscore. They are easier to type, read, and to access from small
unittests. Lint warnings take care of invalid access to protected members.


<a id="s3.16-main"></a>
<a id="316-main"></a>

<a id="main"></a>
### 3.16 Main 

Even a file meant to be used as an executable should be importable and a mere
import should not have the side effect of executing the program's main
functionality. The main functionality should be in a `main()` function.

In Python, `pydoc` as well as unit tests require modules to be importable. Your
code should always check `if __name__ == '__main__'` before executing your main
program so that the main program is not executed when the module is imported.

```python
def main():
    ...

if __name__ == '__main__':
    main()
```

All code at the top level will be executed when the module is imported. Be
careful not to call functions, create objects, or perform other operations that
should not be executed when the file is being `pydoc`ed.
