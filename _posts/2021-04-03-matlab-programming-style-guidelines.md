---
title: "Matlab Programming Style Guidelines"
excerpt: 'A summary of Matlab programming style'
categories:
    - note
tags:
    - coding style
    - matlab
author_profile: true
toc: true
---
本文是Richard Johnson的[文章](https://www.ee.columbia.edu/~marios/matlab/MatlabStyle1p5.pdf)的个人总结，目的是help produce code that is more likely to be correct, understanble, sharable and maintainable.

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

