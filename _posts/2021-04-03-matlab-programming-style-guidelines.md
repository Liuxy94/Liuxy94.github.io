---
title: "Matlab Programming Style Guidelines"
excerpt: 'A summary of Matlab programming style'
categories:
    - note
tags:
    - coding style
    - matlab
toc: true
toc_sticky: false
author_profile: true
---
本文是Richard Johnson的[文章](https://www.ee.columbia.edu/~marios/matlab/MatlabStyle1p5.pdf)的个人总结，目的是help produce code that is more likely to be correct, understanble, sharable and maintainable.<br>
另外这是一个不错的快速上手 Markdown 的[网站](https://commonmark.org/).<br>
>You got to know the rules before you can break them. otherwise, it's no fun. Sonny Crockett in Miami Vice.

## Naming Conventions

### Variables
1. Variable names should be in mixed case starting with lower case. <br>
`linearity`, `credibleThreat`, `quakityOfLife`
2. Variables with a large scope should have meaningful names. Variables with a small scope can have short names. 
3. The prefix n should be used for variables representing the number of objects. <br>
`nFiles`, `nSegments`
4. A convention on pluralization should be followed consistently. <br>
 A suggested practice is to make all variable names either singular or plural. Having two variables with names differing only by a final letter s should be avoided. An acceptable alternative for the plural is to use the suffix Array, like `point, pointArray `.<br>
5. Iterator variables should be named or prefixed with i, j, k etc.
```matlab
for iFile = 1:nFiles
    for jPosition = 1:nPositions
    end
end
```
6. Negated boolean variable names should be avoided. <br>
Use `isFound'` Avoid  `isNotFound`.

### Constants
1. Named constants (including globals) should be all uppercase using underscore to seperate words.<br>
`MAX_ITERATIONS`,  `COLOR_RED`

### Stuctures
1. Structure names should begin with a capital letter.<br>
It helps to distinguish between structures and ordinary variables.<br>
2. The name of the structure is implicit, and need not be included in a fieldname.<br>
Use `Segment.length`<br>
Avoid `Segment.segmentlength`

### Functions
1. Names of functions should be written in lower cae.<br>
`compute_total_width(.)`
2. Functions should have meaningful names.<br>
3. Functions with a single output can be named for the output.<br>
`mean(.)`,  `standarderror(.)`
4. Functions with no output argument or which only return a handle should be named after waht they do. <br>
`plot(.)`
5. Prefiexs: get/set, compute, find, initialize, is. <br>
`getobj(.)`,  `setappdata(.)`,  `computeweightedaverage()`, `findoldestrecord()`, `initializeproblemstate()`,  `isoverpriced(.)`

### General
1. Names of dimensioned variables and constants should usually have a units suffix.
2. Abbreviations in names should be avoided.
3. Consider making names pronounceable.

## Files and Organization

### M Files
1. Modularize
2. Make interaction clear.<br>
A function interacts with other code through input and output arguments and global variables. <strong>The use of arguments is almost always clearer than the use of globals.</strong> Structures can be used to avoid long lists of input or output arguments.
3. Partitioning<br>
All subfunctions and many functions should do one thing very well. Every function should hide something.<br>
4. Use existing functions. <br>
5. Any block of code appearing in more than one m-file should be considered for packaging as a fucntion. <br>
6. Subfunctions <br>
A function used by <strong>only one other function</strong> should be packaged as its subfunction in the same file. This makes the code easier to understand and maintain. <br>
7. Test scripts <br>
Write a test script for every function. This practice will improve the quality of the initial version
and the reliability of changed versions. Consider that any function too difficult to test is probably
too difficult to write. 
> Boris Beizer: “More than the act of testing, the act of designing tests is one of the best bug preventers known.” 

### Input and Output
1. Make input and output modules. <br>
2. Format output for easy use. <br>
If the output will most likely be read by a human, make it self descriptive and easy to read. <br>
If the output is more likely to be read by software than a person, make it easy to parse. <br>
If both are important, make the output easy to parse and write a formatter function to produce a human readable version. <br>

## Statements

### Variables and constatnts
1. Variables should not be reused unless required by memory limitation. <br>
2. Related variables of the same type can be declared in a common statement. Unrelated variables should not be declared in the same statement. <br>
    It enhances readability to group variables.
```matlab
persistent x, y, z
global REVENUE_JANUARY, REVENUNE_FEBRUARY
```
3. Consider documenting important variables in comments near the start of the file. <br>
It is standard practice in other languages to document variables where they are declared. Since
MATLAB does not use variable declarations, this information can be provided in comments. <br>
```matlab
% pointArray Points are in rows with coordinates in columns. 
```
4. Consider documenting constant assignments with end of line comments. <br>
This gives additional information on rationale, usage or constraints.  <br>
```matlab
THRESHOLD = 10; % Maximum noise level found by experiment. 
```

### Global
1. Use of global variables should be minimized. <br>
Clarity and maintainability benefit from argument passing rather than use of global variables.
Some use of global variables can be replaced with the cleaner `persistent` or with
`getappdata`. 

2. Use of global constants should be minimized. <br>
Use an m-file or mat file. This practice makes it clear where the constants are defined and
discourages unintentional redefinition. If the file access is undesirable, consider using a structure
of global constants. 

### Loops
1. Loop variables should be initialized immediately before the loop. <br>
This improves loop speed and helps prevent bogus values if the loop does not execute for all
possible indices. 
```matlab
result = zeros(nEntries,1);
for index = 1:nEntries
    result(index) = foo(index);
end 
```
2. The use of break and continue in loops should be minimized.  <br>
These constructs can be compared to goto and they should only be used if they prove to have
higher readability than their structured counterpart. <br>
3. The end lines in nested loops can have comments <br>
Adding comments at the end lines of long nested loops can help clarify which statements are in
which loops and what tasks have been performed at these points. <br>

### Conditionals
1. Complex conditional expressions should be avoided. Introduce temporary logical
variables instead. <br>
By assigning logical variables to expressions, the program gets automatic documentation. The
construction will be easier to read and to debug. 
```matlab
if (value>=lowerLimit)&(value<=upperLimit)&~ismember(value,…
valueArray)
:
end
```
should be replaced by:
```matlab
isValid = (value >= lowerLimit) & (value <= upperLimit);
isNew = ~ismember(value, valueArray); 
if (isValid & isNew)
:
end
```
2. The usual case should be put in the if-part and the exception in the else-part of an if else
statement.  <br>
3. The conditional expression if 0 should be avoided, except for temporary block
commenting. <br>
4. A switch statement should include the otherwise condition. <br>
Leaving the `otherwise` out is a common error, which can lead to unexpected results.
```matlab
switch (condition)
case ABC
    statements;
case DEF
    statements;
otherwise
    statements;
end 
```
5. The switch variable should usually be a string. 

### General
1. Avoid cryptic code. <br>
Writing concise code can be a way to explore the features of the language. However, in almost every circumstance,
clarity should be the goal. As Steve Lord of TMW has written, “A month from now, if I look at this code, will I understand what it’s doing?” <br>
2. Use parentheses. <br>
If there might be any doubt, use parentheses to clarify expressions. They are particularly helpful for extended logical expressions. <br>
3. The use of numbers in expressions should be minimized. Numbers that are subject to change usually should be named constants instead. <br>
If a number does not have an obvious meaning by itself, readability is enhanced by introducing a named constant instead. It can be much easier to change the definition of a constant than to find and change all of the relevant occurrences of a literal number in a file. <br>
4. Floating point constants should always be written with a digit before the decimal point. <br>
This adheres to mathematical conventions for syntax. Also, 0.5 is more readable than .5; it is not
likely to be read as an integer.<br>
Use `THRESHOLD = 0.5` <br>
Avoid `THRESHOLD = .5`<br>
5. Floating point comparisons should be made with caution. <br>
```matlab
shortSide = 3;
longSide = 5;
otherSide = 4;
longSide^2 == (shortSide^2 + otherSide^2)
ans =
1
scaleFactor = 0.01;
(scaleFactor*longSide)^2 == ((scaleFactor*shortSide)^2 + …
(scaleFactor*otherSide)^2)
ans =
0 
```

## Layout, Comments and Documentation 

### Layout
1. Content should be kept within the first 80 columns. <br>
2. Lines should be split at graceful points. <br>
Split lines occur when a statement exceeds the suggested 80 column limit. <br>
In general:<br>
* Break after a comma or space.
* Break after an operator.
* Align the new line with the beginning of the expression on the previous line. 
```matlab
totalSum = a + b + c + …
d + e;
function (param1, param2,…
param3)
setText ([‘Long line split’ …
‘into two parts.’]); 
```
3. Basic indentation should be 3 or 4 spaces.  (use tab) <br>
4. In general a line of code should contain only one executable statement. <br>
5. Short single statement if, for or while statements can be written on one line. <br>
This practice is more compact, but it has the disadvantage that there is no indentation format cue. <br>
```matlab
if(condition), statement; end
while(condition), statement; end
for iTest = 1:nTest, statement; end
```
### White Space
1. Surround =, &, and | by spaces. <br>
`simplesSum = firstTerm+secondTerm;`
2. Conventional operators can be surrounded by spaces.
This practice is controversial. Some believe that it enhances readability.
```matlab
simpleAverage = (firstTerm + secondTerm) / two;
1 : nIterations 
```
3. Commas can be followed by a space.
These spaces can enhance readability. Some programmers leave them out to avoid split lines.<br>
`foo(alpha, beta, gamma)`<br>
4. Semicolons ';' or commas ',' for multiple commands in one line should be followed by a space
character. <br>
5. Keywords should be followed by a space. <br>
This practice helps to distinguish keywords `false` from functions. <br>
6. Logical groups of statements within a block should be separated by one blank line. <br>
7. Blocks should be separated by more than one blank line. <br>
Code alignment can make split expressions easier to read and understand. This layout can also
help to reveal errors.
```matlab
weightedPopulation = (doctorWeight * nDoctors) + …
(lawyerWeight * nLawyers) + …
(chiefWeight * nChiefs); 
```

### Comments
1. Comments cannot justify poorly written code.
Comments cannot make up for code lacking appropriate name choices and an explicit logical
structure. Such code should be rewritten. Steve McConnell: “Improve the code and then
document it to make it even clearer.”
2. Comments should agree with the code, but do more than just restate the code.
A bad or useless comment just gets in the way of the reader. N. Schryer: “If the code and the
comments disagree, then both are probably wrong.” It is usually more important for the comment
to address why or how rather than what.
3. Comments should be easy to read.
There should be a space between the % and the comment text. Comments should start with an
upper case letter and end with a period.
4. Comments should usually have the same indentation as the statements referred to.
This is to avoid having the comments break the layout of the program. End of line comments tend
to be cryptic and should be avoided except for constant definitions.
5. Function header comments should support the use of help and lookfor.
help prints the first contiguous block of comment lines from the file. Make it helpful.
lookfor searches the first comment line of all m-files on the path. Try to include likely search
words in this line.
6. Function header comments should discuss any special requirements for the input arguments.
The user will need to know if the input needs to be expressed in particular units or is a particular
type of array.
```matlab
% ejectionFraction must be between 0 and 1, not a percentage.
% elapsedTimeSeconds must be one dimensional.
```
7. Function header comments should describe any side effects.
Side effects are actions of a function other than assignment of the output variables. A common
example is plot generation. Descriptions of these side effects should be included in the header
comments so that they appear in the help printout.
8. In general the last function header comment should restate the function line.
This allows the user to glance at the help printout and see the input and output argument usage.
9. Writing the function name using uppercase in the function header is controversial.
This is a MathWorks practice, which is intended to make the function name prominent. Many
other authors do not follow this practice. Its disadvantage is that in code the function name should
usually be in lower case.
10. Avoid clutter in the help printout of the function header.
It is common to include copyright lines and change history in comments near the beginning of a
function file. There should be a blank line between the header comments and these comments so
that they are not displayed in response to help.

### Documentation
1. Formal documentation.<br>
To be useful documentation should include a readable description of what the code is supposed to
do (Requirements), how it works (Design), which functions it depends on and how it is used by
other code (Interfaces), and how it is tested. For extra credit, the documentation can include a
discussion of alternative solutions and suggestions for extensions or maintenance.
Dick Brandon: “Documentation is like sex; when it's good, it's very, very good, and when it's bad,
it's better than nothing.”
2. Consider writing the documentation first.<br>
Some programmers believe that the best approach is “Code first and answer questions later.”
Through experience most of us learn that developing a design and then implementing it leads to a
much more satisfactory result.
Development projects are almost never completed on schedule. If documentation and testing are
left for last, they will get cut short. Writing the documentation first assures that it gets done and
will probably reduce development time.
3. Changes.<br>
The professional way to manage and document code changes is to use a source control tool. For
very simple projects, adding change history comments to the function files is certainly better than
nothing. <br>
`% 24 November 1971, D.B. Cooper, exit conditions modified. `

## References
[MATLAB Programming Style Guidelines][1], Richard Johnson<br>
The Practice of Programming, Brian Kernighan and Rob Pike<br>
The Pragmatic Programmer, Andrew Hunt, David Thomas and Ward Cunningham<br>
[Java Programming Style Guidelines][2], Geotechnical Software Services<br>
Code Complete, Steve McConnel - Microsoft Press<br>
[C++ Coding Standard][3], Todd Hoff<br>

[1]: https://www.ee.columbia.edu/~marios/matlab/MatlabStyle1p5.pdf
[2]: http://docshare02.docshare.tips/files/26690/266901746.pdf
[3]: http://132.248.182.159/utils/CyC++/CyC++/CyC++Style/c++_coding_standards.pdf
