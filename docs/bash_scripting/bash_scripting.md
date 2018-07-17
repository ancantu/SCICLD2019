# BASH SCRIPTING

### Course Objectives

This course is designed as an introduction to managing data on high performance computing clusters. It is intended to teach users to be self-sufficient and proactive in managing their own data in order to (1) increase their productivity, (2) maximize the usage of the existing storage, (3) decrease accidental or unintentional data loss, and (4) prevent disruption of the file system or compute nodes to the point where it may affect others. This course is intended for people who have beginner to intermediate familiarity with a command line interface, and have active accounts on a TACC HPC cluster.

This course is designed to build upon [Introduction to Linux](docs/intro_to_linux/intro_to_linux_01.md) and [Command Line Utilities](docs/intro_to_hpc/intro_to_hpc_01.md)

This course is divided into 2 sections of 6 modules:

 [Intermediate BASH](#intbash)
   1. [BASH Scripts](#ib_mod1)
   2. [Variables](#ib_mod2)
   3. [Arithmetic](#ib_mod3)
   4. [Strings](#ib_mod4)
   5. [Arguments](#ib_mod5)
   6. [Arrays](#ib_mod6)

<br>

[Advanced BASH](#advbash)
   1. [Loops](#ab_mod1)
   2. [IF/THEN/ELSE](#ab_mod2)
   3. [WHILE](#ab_mod3)
   4. [FOR](#ab_mod4)
   5. [CASE](#ab_mod5)
   6. [Subprograms/Functions](#ab_mod6)


### Instructional Objectives

This course should be taught in a room equipped with computers (or attendees with laptops) and internet access. Attendees should also have an existing allocation on a TACC resource. Attendees without an allocation can still participate in most components if they have a Mac / Linux laptop, or a Windows laptop with Putty installed and access to a Linux server.


### Specific Learning Objectives
[Module1](bash_01_01.md)

| <a name="ib_mod1"></a>Module 1: `bash` script basics|
|-|
| |
|Topics covered in this module:|
| • The basic format and construction of `bash` scripts


[Module 1](bash_01_01.md)
<table><tbody>
<tr><th><a name="ib_mod1"></a>Module 1: `bash` script basics</th></tr>
<tr><td></td></tr>
<tr><th> <strong>Topics covered in this module:</strong> </th></tr>
<tr><td><ul><li> The basic format and construction of `bash` scripts </li></ul> </td></tr>
<tr><th> <strong>Attendees should be able to...</strong> </th></tr>
<tr><td><ul><li> Use `vi` to create a script </li><li> Make the script executable using `chmod` </li></ul> </td></tr>
</tbody></table>

<br>

[Module 2](bash_01_02.md)
<table><tbody>
<tr><th><a name="ib_mod2"></a>Module 2: `bash` VARIABLES </th></tr>
<tr><td></td></tr>
<tr><th> <strong>Topics covered in this module:</strong> </th></tr>
<tr><td><ul><li> The types and usage of variables and parameters </li></ul> </td></tr>
<tr><th> <strong>Attendees should be able to...</strong> </th></tr>
<tr><td><ul><li> Identify and know the limits of different variables types in `bash` </li></ul> </td></tr>
</tbody></table>

<br>
[Module 3](bash_01_03.md)
<table><tbody>
<tr><th><a name="ib_mod3"></a>Module 3: ARITHMETIC </th></tr>
<tr><td></td></tr>
<tr><th> <strong>Topics covered in this module:</strong> </th></tr>
<tr><td><ul><li> Arithmetic operations for both integer and floatng point variables </li></ul> </td></tr>
<tr><th> <strong>Attendees should be able to...</strong> </th></tr>
<tr><td><ul><li>Perform basic arithmetic operations on variables</li></ul> </td></tr>
</tbody></table>

<br>
[Module 4](bash_01_04.md)
<table><tbody>
<tr><th><a name="ib_mod4"></a>Module 4: STRINGS</th></tr>
<tr><td></td></tr>
<tr><th> <strong>Topics covered in this module:</strong> </th></tr>
<tr><td><ul><li> The construction and usage of character strings, substrings, and string parameters </li></ul> </td></tr>
<tr><th> <strong>Attendees should be able to...</strong> </th></tr>
<tr><td><ul><li>Extract or modify the value of a string or substring</li></ul> </td></tr>
</tbody></table>

<br>
[Module 5](bash_01_05.md)
<table><tbody>
<tr><th><a name="ib_mod5"></a>Module 5: COMMAND LINE ARGUMENTS</th></tr>
<tr><td></td></tr>
<tr><th> <strong>Topics covered in this module:</strong> </th></tr>
<tr><td><ul><li>Parameters that reference, control, and alter command line arguments for a script.</li></ul> </td></tr>
<tr><th> <strong>Attendees should be able to...</strong> </th></tr>
<tr><td><ul><li> Identify and extract information passed to or from a `bash` script</li></ul> </td></tr>
</tbody></table>

<br>
[Module 6](bash_01_06.md)
<table><tbody>
<tr><th><a name="ib_mod6"></a>Module 6: ARRAYS</th></tr>
<tr><td></td></tr>
<tr><th> <strong>Topics covered in this module:</strong> </th></tr>
<tr><td><ul><li> How to create and modify collections of common elements that form array. </li></ul> </td></tr>
<tr><th> <strong>Attendees should be able to...</strong> </th></tr>
<tr><td><ul><li>Specify and modify entire arrays or just individual array elements </li></ul> </td></tr>
</tbody></table>

<br>
[Module 7](bash_02_01.md)
<table><tbody>
<tr><th><a name="ab_mod1"></a>Module 7: `bash` LOOPS overview </th></tr>
<tr><td></td></tr>
<tr><th> <strong>Topics covered in this module:</strong> </th></tr>
<tr><td><ul><li> The different types and purposes of common `bash` loops. </li></ul> </td></tr>
<tr><th> <strong>Attendees should be able to...</strong> </th></tr>
<tr><td><ul><li> Understand the role of different loop types. </li></ul> </td></tr>
</tbody></table>

<br>
[Module 8](bash_02_02.md)
<table><tbody>
<tr><th><a name="ab_mod2"></a>Module 8: IF/THEN/ELSE</th></tr>
<tr><td></td></tr>
<tr><th> <strong>Topics covered in this module:</strong> </th></tr>
<tr><td><ul><li> The syntax and role for conditional comparisons.</li></ul> </td></tr>
<tr><th> <strong>Attendees should be able to...</strong> </th></tr>
<tr><td><ul><li> Construct scripts that compare variables </li></ul> </td></tr>
</tbody></table>

<br>
[Module 9](bash_02_03.md)
<table><tbody>
<tr><th><a name="ab_mod3"></a>Module 9: WHILE LOOPS</th></tr>
<tr><td></td></tr>
<tr><th> <strong>Topics covered in this module:</strong> </th></tr>
<tr><td><ul><li>The syntax and role for unbound loops</li></ul> </td></tr>
<tr><th> <strong>Attendees should be able to...</strong> </th></tr>
<tr><td><ul><li> Construct scripts that perpetually run until a condition is met. </li></ul> </td></tr>
</tbody></table>

<br>
[Module 10](bash_02_04.md)
<table><tbody>
<tr><th><a name="ab_mod4"></a>Module 10: FOR LOOPS</th></tr>
<tr><td></td></tr>
<tr><th> <strong>Topics covered in this module:</strong> </th></tr>
<tr><td><ul><li> The syntax and role for loops the over lists and ranges </li></ul> </td></tr>
<tr><th> <strong>Attendees should be able to...</strong> </th></tr>
<tr><td><ul><li> Construct scripts the execute commands for each member of a list or range </li></ul> </td></tr>
</tbody></table>

<br>
[Module 11](bash_02_05.md)
<table><tbody>
<tr><th><a name="ab_mod5"></a>Module 11: CASE STATEMENTS</th></tr>
<tr><td></td></tr>
<tr><th> <strong>Topics covered in this module:</strong> </th></tr>
<tr><td><ul><li>How to assess when the values of a looped list are known and what actions should be taken.</li></ul> </td></tr>
<tr><th> <strong>Attendees should be able to...</strong> </th></tr>
<tr><td><ul><li>Design scripts that allow them to contruct hierarchical menu-like responses. </li></ul> </td></tr>
</tbody></table>

<br>
[Module 12](bash_02_06.md)
<table><tbody>
<tr><th><a name="ab_mod6"></a>Module 12: SUBFUNCTIONS</th></tr>
<tr><td></td></tr>
<tr><th> <strong>Topics covered in this module:</strong> </th></tr>
<tr><td><ul><li> creation of reusable code blocks</li></ul> </td></tr>
<tr><th> <strong>Attendees should be able to...</strong> </th></tr>
<tr><td><ul><li> Construct scripts that reuse common repetitive functions </li></ul> </td></tr>
</tbody></table>


<br>

Prev [BASH scripting](bash_scripting.md) | Next [BASH scripts](bash_01_02.md) | UP : [BASH scripting](bash_scripting.md) | Top : [Course Overview](../../index.md)

<br>
&copy; 2017 Texas Advanced Computing Center
