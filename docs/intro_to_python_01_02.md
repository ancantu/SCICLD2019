# Processing Tabular Data Part 1 - Variables & Memory

**Objectives:**
- "Assign values to variables."

**Keypoints:** 
- "Use `variable = value` to assign a value to a variable in order to record it in memory."
- "Variables are created on demand whenever a value is assigned to them."
- "Use `print(something)` to display the value of `something`."

---
In this lesson we will learn how to manipulate the inflammation dataset with Python. But before we discuss how to deal with many data points, we will show how to store a single value on the computer.

The line below **assigns** the value `55` to a **variable** `weight_kg`:

~~~
weight_kg = 55
~~~

A **variable** is just a name for a value,
such as `x`, `current_temperature`, or `subject_id`.
Python's variables must begin with a letter and are [case sensitive](reference.html#case-sensitive).
We can create a new variable by **assigning** a value to it using `=`.
When we are finished typing and press Shift+Enter, the notebook runs our command.

Once a variable has a value, we can print it to the screen:

~~~
print(weight_kg)
~~~

and do arithmetic with it:

~~~
print('weight in pounds:', 2.2 * weight_kg)
~~~

As the example above shows, we can print several things at once by separating them with commas.

We can also change a variable's value by assigning it a new one:

~~~
weight_kg = 57.5
print('weight in kilograms is now:', weight_kg)
~~~

If we imagine the variable as a sticky note with a name written on it,
assignment is like putting the sticky note on a particular value:

![Variables as Sticky Notes](http://swcarpentry.github.io/python-novice-inflammation/fig/python-sticky-note-variables-01.svg)

This means that assigning a value to one variable does *not* change the values of other variables.
For example,
let's store the subject's weight in pounds in a variable:

~~~
weight_lb = 2.2 * weight_kg
print('weight in kilograms:', weight_kg, 'and in pounds:', weight_lb)
~~~

![Creating Another Variable](http://swcarpentry.github.io/python-novice-inflammation/fig/python-sticky-note-variables-02.svg)

and then change `weight_kg`:

~~~
weight_kg = 100.0
print('weight in kilograms is now:', weight_kg, 'and weight in pounds is still:', weight_lb)
~~~



![Updating a Variable](http://swcarpentry.github.io/python-novice-inflammation/fig/python-sticky-note-variables-03.svg)

Since `weight_lb` doesn't "remember" where its value came from,
it isn't automatically updated when `weight_kg` changes.
This is different from the way spreadsheets work.

# Who's Who in Memory

You can use the `%whos` command at any time to see what variables you have created and what modules you have loaded into the computer's memory. As this is an IPython command, it will only work if you are in an IPython terminal or the Jupyter Notebook.

~~~
%whos
~~~

~~~
Variable    Type       Data/Info
--------------------------------
numpy       module     <module 'numpy' from '/Us<...>kages/numpy/__init__.py'>
weight_kg   float      100.0
weight_lb   float      126.5
~~~

