# 
<h1 style="text-align: center">CS-137 Programming Principles</h1>
<h1 style="text-align: center">Notes by Daniel Prilik</h1>
<h1 style="text-align: center">Software Engineering Class of 2020</h1>

## A note about these notes
They aren't *amazing,* but they are pretty solid.
I *may* have missed a thing or two here and there, but i've tried not to miss any super important key concepts.
Also, brace yourself if you hate spelling errors. There are going to be *quite* a few (probably).

These note do not have any program flow diagrams, partly because I didn't find them useful, but mostly because it would be a pain to actually write in markdown.

One last disclaimer: I give you no guarantees that these notes are accurate, so use them at your own risk.

That said, enjoy!

## Office Hours
**12:30 - 1:30 pm** on Wednesdays

* * *

***Table of Contents:***

<!--Insert Table of Contents below...-->

* * * *

## Lecture 1
Sept 14th 2015

### Outline of the course

1. Basic C programming concepts
    a) variables
    b) integers, chars, expressions
    c) conditionals
    d) loops
2. Functions, parameters, arguments, and recursion
3. Arrays and Pointers
4. Structures
5. Sorting, searching, time and spave complexity 	// ***not covered in textbook***
6. Other fun stuff :) 								// ***not covered in textbook***

Core CS sequence
 Term | Course | Name                            | Language
 ---- | ------ | ------------------------------- | -----------
  1A  | CS 137 | Programming Principles          | (C)
  1B  | CS 138 | Data Abstraction                | (C++)
  2A  | CS 241 | Sequential Programs             | (MIPS, …)
  2B  | CS 247 | Abstraction and Specification   | (more C++)

Supplementary courses
Course  | Name
------- | -------------------------------
 SE 101 | Foundations of Software Engineering
ECE 124 | Arduino stuff
 SE 212 | Proving program correctness
ECE 222 | How processors work
 CS 240 | Data Structures
 CS 341 | Algorithms

### First C Program:
#### *hello.c*
```
    #include <stdio.h>

    int main(void) {
        printf("Hello World!\n");
        return 0;
    }
```

You can compile c programs on linux by running: **gcc [-o (name of exe) ] (file *.c)**

* * *

## Lecture 2
Sept. 16th 2015

### Using Euclid's algorithm to find the Greatest Common Divisor (GCD)

First, we need two values:

$$
x = 338 \\
y = 806
$$

Now, we subtract one value from the other:

$$
| x  - y | =  468
$$

Somehow, we need to find a number that can evenly divide both x and x-y

That's hard with big numbers, so instead, we make the problem smaller. 

First, we set $$$ y $$$ to be the abs difference of x and y:

$$
y = | x - y | = 468
$$

Then, We simply repeat the algorithm on $$$ x $$$ and $$$ y $$$ again!

$$
x = 338, y = 468 \\
| x  - y | =  130 = y\\
$$

And again...

$$
x = 338, y = 130 \\
| x  - y | =  208  = y \\
$$

And repeat ad-nauseum until you get $$$ | y-x | = 0 $$$

In this case, the x and y values that subtract to 0 are 26, thus we know 26 is the GCD.

Using the MOD operator makes this process go faster, as it skips numbers.
*sidenote*: The mod operator returns the remainder between a quotient of 2 int's

$$
806\ \%\ 338 = 130 \\
338\ \%\ 130 = 78 \\
130\ \%\ 78 = 52 \\
78\ \%\ 52 = 26 \\
52\ \%\ 26 = 0
$$

And, there we have it.
When using the MOD method, the GCD is the smaller number in the final MOD operation (in this case, 26).

This algorithm can be simply implemented in c as so:

----------------

####*gcd.c*
```
    #include <stdio.h>

    int main(void) {
        int a = 806;    // Number 1
        int b = 338;    // Number 2
        int r = 0;      // Temporary storage var

        while (b!=0) {
            r = a % b;
            a = b;``
            b = r;
        }
        printf("%d\n", a);
        return 0;
    }
```

----------

***I proceeded to miss some notes about printf and scanf. Whoops.***

* * *

## Lecture 3
Sept 18th

### Computer Memory

Computer memory is essentially a table of bytes
Index | Value of Byte |
----  | ------------- |
0     |   0010 0101   |
1     |   0110 1101   |
2     |   0100 1001   |
...   |               |

A single **Byte** (Binary Digit) is **8 Bits**

Place Value |128|64 |32 |16 | 8 | 4 | 2 | 1 
------------|---|---|---|---|---|---|---|---
Digit Value | 0 | 0 | 1 | 0 | 0 | 1 | 1 | 0

*Typically*, **4** bytes makes an ***int*** type

*Sidenote*: The number above would be 32 + 4 + 2 = 38~10~

Index | Byte 1    | Byte 2    | Byte 3    | Byte 4    |
----  | --------- | --------- | --------- | --------- |
0     | 0010 0100 | 0000 0010 | 0100 0010 | 0100 0101 |
1     | 0000 0010 | 0100 1000 | 0100 0100 | 1001 1001 |
2     | 1000 1000 | 0010 0101 | 0011 0010 | 0100 0100 |
...   | 0010 0010 | 1000 1000 | 1000 1000 | 0001 0001 |

This gives every index a total of **32 Bits** with which to store info!

### Variable types in C
Following are the typical sizes for variables in C:
 Type                   | Bits     | Bytes   | Value Range
 ---------------------- | -------- | ------- | -----------------------------------
 char                   | 8        | 1       | [-128, +127] i.e [-2^7^, +2^7^ - 1]
 int                    | 32       | 4       | [-2^31^, +2^31^ - 1]
 unsigned char          | 8        | 1       | [0, +255]
 short int              | 16       | 2       | [-2^15^, 2^15^ - 1]
 unsigned short int     | 16       | 2       | [0, +2^16^ - 1]
 unsigned int           | 32       | 4       | [0, +2^32^ - 1]
 long int               | 32 or 64 | 4 or 8  |
 long long int          | 64       | 8       | [-2^63^, +2^63^ - 1]
 unsigned long long int | 64       | 8       | [0, +2^64^ - 1]

### Unsigned integers
An **8 bit** number has the following memory layout:
Place Value | 2^7^ | 2^6^ | 2^5^ | 2^4^ | 2^3^ | 2^2^ | 2^1^ | 2^0^ |
------------|------|------|------|------|------|------|------|------|
Digit Value | b~7~ | b~6~ | b~5~ | b~4~ | b~3~ | b~2~ | b~1~ | b~0~ |

The value of this byte in binary would be $$$ b_7 \cdot 2^7 + b_6 \cdot 2^6 + ... + b_0 \cdot 2^0 $$$

***Examples***:
 * 0000 0000~2~ = 0~10~
 * 0101 0101~2~ = 1+4+16+64 = 85~10~
 * 1111 1111~2~ = 1+2+4+8+16+32+64+128 = 255~10~

**Unsigned integers** use all 8 bits of their memory to store a value, as opposed to **Signed integers**, that only use the last 7 bits of their memory to store a value.

### Signed integers
Negative values are represented with **two's complement** of their absolute value's binary representation.

In two's complement, the leftmost bit is the **signing bit**, and the remaining bits are used to store the value of the number.

If the signing bit is **0**, the number is **positive**.
If the signing bit is **1**, the number is **negative**.

But how do we actaully convert a negative number in Base 10 into a signed integer stored in Base 2? We follow these steps:
1. start with the unsigned binary
2. complement each bit
3. add 1

***Example***:
How do we represent -38~10~ in binary?

First, we take it's absolute value, and represent it in Binary:
$$
    38_{10} \\
    \Downarrow\\
    0010\ 0110_2\\
$$
Then, we turn each 1 into a zero, and each 0 into a 1
$$
	0010\ 0110\\
    \Downarrow\\
    1101\ 1001
$$
We now have **1's compliment**, which is useless except to later get **2's complement.**

To get 2's compliment, we add 1 to 1's compliment:
$$
    1101\ 1001_2 + 1 = 1101\ 1010_2
$$
Thus, we see that $$$ -38_{10} $$$ can be expressed as $$$ 1101\ 1010_2 $$$

### Expressions in C
Let $$$ a = 806 $$$ and $$$ b = 338 $$$

 Function | Operator | Example
----------|----------|---------
 add      | +        | a + b = 1144
 subtract | -        | a - b = 468
 multiply | *        | a * b = 272428
 divide   | /        | a / b = 2 ( NOT 2.385 ) -- Ints truncate
 modulus  | %        | a % b = 130 ( remainder )

### Order of Evaluation
C follows the basic elementary school rules of **BEDMAS**
C evaluates (in order): brackets, exponents, division, multiplication, adding, subtracting

so,that means that: `a / b + a * b` $$$ \Longleftrightarrow $$$ `(a / b) + (a * b)`

* * *

## Lecture 4
Sept 21st

### Book-keeping
**Quiz 0** will be on Friday, at the start of class. It will be about **order of operations**
Eg:
```
	a = 1;
    b = 2;
    c = 3;

    printf("%d\n", a - b * c);
```
***Output:*** -5

**Assignment 1** will be released **tomorow**, and will be due in a **week**.

### Assignment
let $$$ a = 806 $$$ and $$$ b = 338 $$$
Take the following code sample:
```
	a = a + b; // 1144
    b = a / b; // 3
    b = b + 1  // 4
```

An interesting thing about the assinment operator: It has a return value!
`c = a = a + b` would set c to 1144, as `a = a + b` returns `a + b`!

### Compund Assignment
We can rewrite `a = a + 2` as `a += 2`
Similarly, for all other operations:
```
	a += 2;
    a -= 2;
    a *= 2;
    a /= 2;
    a %= 2;
```

### Increment and Decrement
We can shorten `a += 1` down even further by rewriting it as either:
1. `++a`	pre-increment (evaluated **before** the rest of the expression)
2. `a++`	post-increment (evaluated **after** the rest of the expression)

Similarly, `a -= 1` can be rewritten as either `a--` or `--a`;

Example of how pre and post decrement matter:
let $$$ a = 806, b = 338 $$$
```
	a += ++b; // a = 1145, b = 339
    a += b++  // a = 1144, b = 2299
    a += --b  // a = 1143, b = 337
    a += b--  // a = 1144, b = 337
```

### Operator Table
For a full table, refer to **Table 4.2**, on **Page 63** of the Textbook
Appendix **A** has a complete table also.

 Precedence | Operator              | Associativity
 ---------- | --------------------- | -------------
 1          | ++, - - (post)        | left
 2          | ++, - - (pre)         | right
            | +, - (unary*)         | right
 3          | *, /, %               | left
 4          | +, - (binary\*\*)     | left
 5          | =, +=, -=, *=, /=, %= | right

\* **Unary** is when you do stuff like `a = -a`;
\*\* **Binary** is when you do stuff like `a = a + b`

Examples:
* Left associativity
	- `i - j - k` $$$ \Longleftrightarrow $$$ `(i - j) - k`
	- `i * j / k` $$$ \Longleftrightarrow $$$ `(i * j) / k`
* Right associativity
	- `a = b = c` $$$ \Longleftrightarrow $$$ ` a = (b = c)`
* Combined example:
	- `3 * b - ++c` $$$ \Longleftrightarrow $$$ `(3 * b) - (++c)`

### Logical Expressions
C, being a stupid, old language, lacks dedicated **boolean variables**.
In C: 
**0** evaluates as **False**
**Non-0** evaluates as **True** (Usually 1)

There are also:
* **Relational Operators:** `<, >, <=, >=`
* **Equality Operators:** `==, !=`
* **Logical Operators:** `!, &&, ||`
	- `||` and `&&` use **short circuited** evaluation, i.e: they always evaluate the left operand, and only evaluate the right operand if necesary.
		* for `&&`: if the **left** is **false**, the **right** is **not evaluated**
		* for `||`: if the **left** is **true**, the **right** is **not evaluated**

	- Examples:
    ```
        (100 > 700) || (2 >= 7)  // is True,  both ops were evaled
        (100 > 700) && (2 >= 7)  // is False, only the left op was evaled
        !(100 > 700) && (2 >= 7) // is False, both ops were evaled
    ```

The relational, equality, and logical operators return **1** if **True**, and **0** if **False**

 `(i >= j) + (i == j)` | relation
 --------------------- | --------
 0                     | `i > j`
 1                     | `i < j`
 2                     | `i == j`

These operators have their own precedence table:
 operator   | precedence            | associativity
 ---------- | --------------------- | -------------
 !          | same as unary +,-     | right
 relational | lower than arithmetic | left
 equality   | lower than relational | left
 logical    | lower than equality   | left

Example: `i + j < k || ~k == 1` $$$ \Longleftrightarrow $$$ `((i+j) < k) || ((!k) == 1)`

## Lecture 5
Sept. 22nd

### Combined Operator Table
 precedence | operator              | associativity
 ---------- | --------------------- | -------------
 1          | ++, -- (post)         | left
 2          | ++, -- (pre)          | right
            | +, - (unary)          | 
            | !, ~                  | 
 3          | *, /, %               | left
 4          | +, - (binary)         | left
 5          | <, <=, >, >=          | left
 6          | =, !=                 | left
 7          | &                     | left
 8          | ^                     | left
 9          | &#124;                | left
 10         | &&                    | left
 11         | &#124; &#124;         | left
 12         | ? :                   | right
 13         | =, +=, -=, *=, /=, %= | right
            | ~=, &=, ^=, &#124;=   | 

### Bitwise operators
let $$$ a = 2_{10}, b = 3_{10} $$$
thus $$$ a = 0000\ 0101_{2}, b = 0000\ 0011_{2} $$$

The bitwise operators do the following:
 Operator | Description | Example
 -------- | ----------- | -------
 ~        | NOT         |`c = ~a` $$$ \Longrightarrow c = 1111\ 1010 $$$
 &        | AND         |`c = a & b` $$$ \Longrightarrow c = 0000\ 0001 $$$
 ^        | XOR         |`c = a ^ b` $$$ \Longrightarrow c = 0000\ 0110 $$$
 &#124;   | OR          |`c = a`  &#124; `b` $$$ \Longrightarrow c = 0000\ 0111 $$$

### DeMorgan's Theourem
DeMorgan's Theourem states that
$$
	!(P\ \&\&\ Q)\ ==\ !P\ ||\ !Q \\
    !(P\ ||\ Q)\ ==\ !P\ \&\&\ !Q
$$

**Proof:**

 P | Q | P && Q | !(P && Q)
 - | - | ------ | ---------
 0 | 0 | 0      | 1
 0 | 1 | 0      | 1
 1 | 0 | 0      | 1
 1 | 1 | 1      | 0

 !P | !Q | !P &#124;&#124; !Q 
 -  | -  | -------------------
 1  | 1  | 1
 1  | 0  | 1
 0  | 1  | 1
 0  | 0  | 0

Example of Application:
` !( i >= 0 && i < n) ` $$$ \Longleftrightarrow $$$ ` ~(i >= 0) || !(i < n) ` $$$ \Longleftrightarrow $$$ `i < 0 || i >= n`

### Selection Statements
In C, there are 2 types of selection statements:
1. if / else
2. switch

We'll simply discuss if / else, read about switch yourself.

If statements come in two forms:

**Single If**
```
	if (expresson)
    	statement
```

**If / Else**
```
	if (expresson)
    	statement
    else
    	statement
```

One can have **nested ifs**, but one must remember that if writing code without brackets, else will always match with the **nearest unmatched if**, ***regardless of indentation.***
Example:
```
	int 0 = 3;
    if (i % 2 == 0)
    	if (i == 0)
        	printf("zero\n");
    else
    	printf("how odd\n");
```
This code actually prints **nothing!** If you add brackets, you see why:
```
	int 0 = 3;
    if (i % 2 == 0) {
    	if (i == 0) {
        	printf("zero\n");
        } else {
            printf("how odd\n");
        }
    }
```

Another interesting thing to note: *C doesn't have an "else if" keyword!*
How does C handle else if statements?
The following example explains:
```
	// Without Brackets vs With Brackets
    
	if (expr)           // if (expr) {
    	// blah         //     // blah
    else if (expr)      // } else {
    	// blah         //     if (expr) {
    else                //         //blah
        // blah         //     } else {
    	                //         // blah
                        //     }
                        // }
```

### Compound statements
**Brace Brackets** make one statement from 0, 1, 2, 3, ... expressions
Example:
```
	if ( i == 0 )
    	printf("zero\n");
    else {
    	printf("even\n");
        printf("or\n");
        printf("odd\n");
    }
```

### Conditional expression
C has this neat expresson: `? :`
It is used in the following way. ` (condition) ? {if true} : {if false} `
Essentially, it is a condensed if statement.

Example:
```
	if (i > j)
    	printf("%d\n", i);
    else
    	printf("%d\n", j);
```
Can be condensed into simply `printf("%d\n", i > j ? i : j )`

### Boolean Values
C origionally did not have boolean values.
True was 1, False was 0.

In the C99 Specification, they added boolean values.

They are used as such:
```
	#include <stdbool.h>
    
    int main (void) {
    	bool first = true;
        
        for (;;) {
            if (true)
                first = false;
            else
                printf("Boop")
        }
        return 0;
    }
```

## Lecture 6
Sept 25th

### While Loop
While loops execute **0 or more** times
Example of a While loop:
```
	int r = a % b;
    while (r != 0) {
    	a = b;
        b = r;
        r = a % b;
    }
```

### Do While Loop
Do While loops execute **1 or more** times
Example of a Do While loop:
```
	do {
    	r = a % b;
        a = b;
        b = r;
    } while (r != 0);
```

### Functions
We can rewrite GCD as a function:
```
	int gcd (int a, int b){
    	int r = a % b;
        while (r != 0) {
        	a = b;
            b = r;
            r = a % b;
        }
        return b;
    }
```

If we define this function in a program, we can then write something like this:
```
	#include <stdio.h>

    int gcd (int a, int b){
    	int r = a % b;
        while (r != 0) {
        	a = b;
            b = r;
            r = a % b;
        }
        return b;
    }

    int main (void) {
    	printf("%d\n", gcd(806, 338));  // prints out 26
        printf("%d\n", gcd(15, 27));    // prints out 3
        return 0;
    }
```
- **int** before gcd is the **return type** of the function
- **int a** and **int b** are called the **parameters** of the function
- **806** and **338** are called the **arguments** passed to the function
- This entire code block is called a **function definition**

***NOTE:*** To use a function, you **MUST** declare it **before** it is invoked!

## Lecture 7
Sept 28th

### Nothing important hopefully...

## Lecture 8
Sept 29th

### Assertions
Assertions can be inplemented by including the `assert.h` header library, and by calling `assert(exp)` somewhere in a program.

If `expr` evaluates to **true**, nothing happens.
If `expr` evaluates to **false**, it terminates the program with a message stating:
* the filename
* line number
* function name and expression

Evidently, `assert` is supremely useful for debugging.

Here is an example relevant to A2_02
```
	#include <assert.h>
    ...
    bool leap(int year) {
    	assert(year >= 1753);
        if (year % 400 == 0) {
        ...etc
    }
```
Asserts can be left in a program even when pushing code in production, and in fact, they should!
- Programmers make assumptions when programming and then forget about them
- Assertions cause a program to fail "loudly" when these assumptions are broken, rather than failing "softly" (continuing to execute regardless of error)
- Asserts protect against future modifications that insert logic errors into the program

### Seperate Compilation
What if we want to compile a program from multiple files?

Say we have 2 c files that depend on one another:

#### *powers.c*
```
	/* Defines the functions */
    int square(int num) { return num * num; }
    int cube(int num) { return square(num) * num; }
    int quartic(int num) { return square(num) * square(num); }
	int quintic(int num) { return cube(num) * square(num); }
```
***Notice the lack of main!***

#### *main.c*
```
	#include <stdio.h>

    /* Below we declare:
         1) That these functions exist
         2) The return type of these functions
         3) The parameter types of the functions
    */
    int quartic(int num); // does not have to have the same
    int quintic(int num); // parameter name as in powers.c

    void testQuartic (int num) { printf("%d^4 = %d\n", num, quartic(num)); }
    void testQuintic (int num) { printf("%d^5 = %d\n", num, quintic(num)); }

    int main() {
    	testQuartic(3);
        testQuintic(2);
    }
```
To compile the two source files together, we have to run a modified `gcc` command:
`gcc -o powers main.c powers.c` (order of source files should not matter)

### Header Files
Header files declare functions (and a whole bunch of other stuff that we don't care about right now.)

For example, the `powers` program above could be rewritten to use a header file
#### powers.h
```
	#ifndef POWERS_H  // For if we accidentally include a duplicate header file
        #define POWERS_H
        int square(int num);
        int cube(int num);
        int quartic(int num);
        int quintic(int num);
    #endif
```

#### powers.c
```
	*** Stays the same ***
```

#### main.c
```
	#include <stdio.h>
    #include "powers.h"

    ... stays the same ...
```
Using a header does not change the compiler arguments, so to compile the two source files together, we still just run: `gcc -o powers main.c powers.c`

## Lecture 9
Oct 2nd

### Recursion
Recursion is having a function call itself.

For example, let's rewrite gcd.c recursively:

####*gcdr.c*
```
	int gcd(int a, int b) {
    	if (b == 0) return a;
        return gcd(b, a % b);
    }
```
Dayum. Ain't that short!

### Pass by Value
Take the following code:
```
	void weird (int n) {
    	n = 17;
    }
    
    void main () {
    	int n = 6;
        weird(n);
        printf("%d\n", n);
    }
```
What do you think it outputs?
It's not `17`! It's actually `6`!

This is because all that weird did was make a locally scoped variable and set it's value. Just because the two variables had the same name, does not mean that one changes the value of the other!

### Arrays
This is a scalar: `int a = 6;`
`6` is the initalializer for the array.

In memory, it looks like this:
 var | val
 --- | ---
 a   | 6

This is an array: `int a[5] = {10, -7, 3, 8, 42};`
`10, -7, 3, 8, 42` are all initializers for a.

In memory, it looks like this:
 var | val
 --- | ---
 a   | 10
     | -7
     | 3
     | 8
     | 42

Size can be set either:
**explicitly**: `int a[5] = {10, -7, 3, 8, 42};`
**implicitly**: `int a[] = {10, -7, 3, 8, 42};`

When defining **implicitly**, C sets the size of the array to the number of elements it was initialized with.

If defining **explicitly**, but only **defining a few of the values**, C fills the rest of the array with 0's
Eg: `int b[5] = {1,2,3} == {1,2,3,0,0};`
Eg: `int c[5] = {0} == {0,0,0,0,0};`

If you don't initialize an array with values, it will still be allocated in memory, but referencing the array will yield garbage...
Eg: `int d[5]; == {?,?,?,?,?}`

Arrays are **0 indexed**.
When referencing an element of an array, C counts the first element as element 0
Eg: `a[3] = {1,2,3}; printf("%d\n", a[2]);` outputs **3**

## Lecture 10
Oct 6th

### Sieve of Eratosthenes
The Sieve of Eratosthenes finds all primes up to a number n (some upper bound)

Eg: Given n=20, the function returns 2,3,5,7,11,13,17,19

The alogorithm works by starting with 2, marking it as prime, and then crossing out all multiples of 2. Once it gets to the upper bound, it goes on to the next non-corssed out number (3), and crosses out all of it's multiples. It continues to do this until it hits a number $$$ \leq \sqrt{n} $$$

<img src="https://upload.wikimedia.org/wikipedia/commons/b/b9/Sieve_of_Eratosthenes_animation.gif">

An implementation in C follows:
####*sieve.c*
```
	#include <stdio.h>
    void sieve(int a[], int n) {
        // a[] is variable sized, so we must also
        // pass n, the size of the array

        if (n <= 0) return; // empty array
        a[0] = 0;
        if (n == 1) return; // just 1
        a[1] = 0;
        if (n == 2) return; // just 1 and 2

        // set the rest of the numbers to 1
        // as they may be prime
        for (int i = 2; i < n; i++) a[i] = 1;

        for (int i = 2; i <= (n-1)/i; i++) {
            if (a[i]) {
                for (int j = 2*i; j < n; j += i) {
                    a[j] = 0;
                }
            }
        }
    }

    void main () {
        int n;
        scanf("%d", &n);
        int a[n+1];
        sieve(a, n+1);

        for (int i = 0; i <= n; i++) {
            if (a[i]) printf("%d\n", i);
        }
    }
```

### Sizeof Operator
`sizeof(type)` Returns the size of the **type** in bytes
`sizeof(var)` Returns the size of the **variable** in bytes
It is usually implemented at runtime (so variable length arrays don't play nicely with it)

Take the following example:
```
	void strange(int a[], int n) {     // a is JUST A POINTER!
    	printf("%d\n", sizeof(a));     // Either 8 (or 4)
        							   // Depends of size of pointer implement
    }
    void main() {
    	int i;
        int a[10] = {0};

        printf("%d\n", sizeof(int));   // 4
        printf("%d\n", sizeof(i));     // 4
    	printf("%d\n", sizeof(char));  // 1
        printf("%d\n", sizeof(a));     // 40 ---- this is a known size arr

        int n = sizeof(a)/sizeof(a[0]); // gives length of arr, 
        								// even if variable length
        stange(a, n);

        int m; scanf("%d", &n); // INPUT 8
        int b[n];
        printf("%d\n", sizeof(b)); //32
    }
```

## Lecture 11
Oct 7th

### Array Pointers
Look at this code:
```
	int a[5] = {0,1,2,3,4};
    printf("%lu\n", (unsigned long)a); // Prints "140737326347974"
```
What the hell is that thing?!

It's a **pointer** to the first item in the array in memory!

Because `a` is just a pointer, we can rewrite functions that take arrays using **pointer notation**, i.e:
`void foo (int a[], int n)` can be rewritten as `void food (int *a, int n)`

Keep in mind that even though you *can* use pointer notations, you *probably shouldn't*, as pointer notation usually obfuscates the nature of the function for those who have to look at it.

Weird things happen when assigning arrays to other variables:
```
	void main () {
    	int a[] = {0,1,2,3,4};
        int *b = a;

        printf("%d\n", a[2]);  // prints 2
        b[2] = 100;
        printf("%d\n", a[2]);  // prints 100
    }
```
a and b point to the **same** memory adress, so any edits to one affect the other.

### Multidimensional arrays
Arrays within Arrays within Arrays, Oh my!
```
	int a[4][3] = {
    	{11,12,13},
        {21,22,23},
        {31,32,33},
        {41,42,43}
     };

     printf("%d\n", a[1][2]); // 23

    int sum = 0;
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 3; j++) {
            sum += a[i][j];
        }
    }

     printf("%d\n", sum);
```

A little caveats of multidimensional arrays:
When defining a function that takes a multidemensional array, one **must** specify **all but the first dimension**!
eg: `int sum(int a[][3][4], int n)` is valid
eg: `int sum(int a[][][4], int n, int m)` is **NOT** valid

### Floating Point Numbers
C stores floating points using Scientific Notation (eg: $$$-2.6302 * 10^{30}$$$)
The memory used in such a number is divided between the **precision** of the number (-2.6302), and the **range** of the number (30).

There are a few different types of floating point numbers:
type        | storage  | precision  | range
----------- | -------  | ---------- | -------
float       | 4 bytes  | ~7 digits  | +- 38
double      | 8 bytes  | ~16 digits | +-308
long double | 16 bytes | ~31 digits | +-4931

Look at the following code for an example of how to use floats:
```
	float z = 1.0f    // use float as type, and f to cast number as float
    double y = 1.0;   // not always neccessary to cast number when assigning

    void main () {
    	double x = -2.6302e30; // yet another way to define floats

        printf("%d\n", sizeof(x)) // 8 [bytes];
        printf("%f\n", x)' // -26302 000 000 000 00 123 564 578 345 000 000

        // there is gibberish in the number due to
        // binary being unable to perfectly represent
        // some decimal numbers

        printf("%.2e\n", x); // -2.63e+30
        // The .2 truncates the float, and e prints it in scientific notation

        printf("%g\n", x); // -2.6302e+30
        // %g prints out whatever is shorter between %e and %f (int this case %g)
    }
```

## Lecture 12
Oct 7th (Again)

### The IEEE 754 Floating Point Standard
Double Precision numbers are 64 bits, with the following layout:
* Bit 63: Sign (1 is negative, 0 is positive)
* Bits 62-52: Exponent
* Bits 51-0: Fraction

Eg: +1.1e2^1 = `0|100 000 000 000|10000...`

Floating points are only *approximations* of real numbers.
Eg: $$$ 0.1_{10} $$$ cannot be represented perfectly in binary. It's binary representation is $$$ 0.0001111..._{2} $$$

### Representaion Error
Let **r** be a **real number**
Let **p** be the **approcimated value**

The **absolute error** is $$$ | p - r | $$$
eg: $$$ |3.14 - \pi| \approx 0.0015927 $$$

The **relative error** is $$$ \frac{|p-r|}{|r|} $$$
eg: $$$ \frac{|3.14-\pi|}{|\pi|} \approx 0.000507 $$$

Relative error can be very large if r is small.
Thus, care is required when:

a) subtracting nearly equal numbers
b) dividing by very small numbers
c) multiplying by very large numbers
d) tests for equality

Example of riskyness:
```
	double x = 5.0/6.0;
    double y = 1.0/2.0;
    double z = 1.0/3.0;

    if (x-y == z) printf("equal\n");
    else printf("not equal\n");

    // Prints 'not equal'
```

Smarter to rewrite it as:
```
	#define EPSILON 0.00001

    // can use fabs() instead of || below
	if (x - y - z < EPSILON || x-y-z > -EPSILON) printf("equal\n");
    else printf("not equal\n");

    // Prints 'equal'
```

## Lecture 13
Oct. 9th

### Polynomials
You can represent polynomials as an array of coefficients.
Ex: $$$f(x) = 3x^3 +4x^2+6$$$ can be represented as  `double f[] = {3,4,0,6}`

Evaluating the polynomial is also pretty easy:

####*poly.c*

```
    #include <stdio.h>
    #include <assert.h>

    double eval_p(double f[], int n, double x) {
        assert(n > 0);

        double y = f[0];
        double p = 1.0;

        for (int i = 0; i < n; i++) {
            p *= x;
            y += f[i]*p;
        }

        return y;
    }

    int main () {
        double f[] = {2,9,4,3};
        printf("%f\n", eval_p(f, 4, 2));

        return 0;
    }
```
This is the intuitive implementation.
There are:
- 2(n-1) **float multplies**
- n-1 **float additions**

Not bad... but there is a more efficient method!
It is called **Horner's Method**

### Horner's Method
A polynomial such as
$$ f(x) = 3x^3+4x^2+9x+2 $$
Can be rewritten as
$$ f(x) = 2+x(9+x(4+3x)) $$

We can leverage this fact to make a more efficient algorithm:
#### *poly_h.c*
```
    #include <stdio.h>
    #include <assert.h>

    double horner(double f[], int n, double x) {
        assert(n > 0);

        double y = f[n-1];

        for (int i = n - 2; i >= 0; i--) {
            y = x*y + f[i];
        }

        return y;
    }

    int main () {
        double f[] = {2,9,4,3};
        printf("%f\n", horner(f, 4, 2));

        return 0;
    }
```

This implementation only has:
- n-1 **float multplies**
- n-1 **float additions**

Dat efficiency doe.

### Math.h Highlights
```
	#include <math.h>

    // Trig
    double sin  (double x); // sine
    	   cos            // cosine
           tan            // tangent
           asin           // sin-1
           acos           // cos-1
           ...            // etc

    // Exponents
    double exp   (double x);           // e^x
    	   log                      // ln x
           log10                    // log x 
           sqrt                     // squrt
           pow   (double x, double y)  // x^y

    // Misc.
    double ceil  (double x);
           floor
           fabs

    // Constants
        M_PI      // pi
        M_PI_2    // pi/2
        M_E       // e
        M_LN2     // ln(2)
        M_SQRT2   // sqrt(2)
        NAN       // not-a-number
        INFINITY  // Guess.
```

## Lecture 14
Oct 14th

### The Bisection Algorithm
We can approximate the roots of a function using the Bisection Algorithm.

Refer to MATH 117 Notes for details on the algorithm.

As a refresher, the algorithm is as follows:

1) Start with some $$$ a $$$ and $$$ b $$$ where $$$ f(a) < 0 $$$ and $$$ f(b) > 0 $$$
2) Compute the midpoint $$$ m = (a+b)/2 $$$
3) if $$$ f(m) < 0 $$$ then $$$ a = m $$$, else $$$ b = m $$$
4) GOTO 2 as many times as one likes for precision

Note: This algorithm only works accurately when there is only **one** root between a and b.

Obviously, we can rewrite this algorithm in C:
This program caluclates the root of the function $$$ f(x) = x - cos(x) $$$
#### *bisect.c*
```
	#include <assert.h>
    #include <math.h>
    #include <stdio.h>

    double f(double x) {
        return x-cos(x);
    }

    double bisect(double a, double b, double epsilon, int max_iter) {
        assert(f(a) * f(b) < 0 && epsilon > 0);
        double m, fm;
        for (int i = 0; i < max_iter; i++) {
            m = (a+b)/2;
            fm = f(m);
            if (fabs(fm) <= epsilon) return m;


            if (f(a)*fm < 0)
                b = m; 
            else 
                a = m;
        }
        return m;
    }

    int main(void) {
        printf("%g\n", bisect(-10, +10, 0.001, 100000));
        return 0;
    }
```

How can we calculate the number of iterations it took to calculate x?
- we start with $$$ \frac{b-a}{2} $$$
- each iteration reduces the distance by half
- Thus, using math...
$$
	\epsilon = \frac{b-a}{2} \cdot \frac{1}{2} \cdot \frac{1}{2} \cdot \frac{1}{2} ... (x\ times) \\
    \epsilon = \frac{b-a}{2} \cdot 2^{-x} \\
    2^x = \frac{b-a}{2\epsilon} \\
    log(2^x) = log(\frac{b-a}{2\epsilon}) \\
    x = \frac{log(\frac{b-a}{2\epsilon})}{log(2)}
$$

In our example above, $$$ x \approx 13 $$$

### Fixed-Point Iteration
FPI is yet another root finding method

Given $$$ f(x) = x - cos(x) $$$, we want $$$ x_0 $$$ such that $$$ f(x_0) = 0 $$$
$$
    x - cos(x) = 0 \\
	g(x) = cos(x)
$$

We start with a rough guess...
$$
	x_{n+1} = g(x_n)
$$
And we repeat until $$$ |x_{n+1} - x{n}|< \epsilon $$$

Note: ** THIS DOES NOT ALWAYS CONVERGE. **

In C, this function is written as:

#### *fixed.c*
```
	#include <assert.h>
    #include <math.h>
    #include <stdio.h>

    double g(double x) {
    	return cos(x);
    }

    double fixed(double guess, double epsilon, int max_iters) {
    	for (int i = 0; i < max_iters; i++) {
        	double gx = g(guess);
            if (fabs(gx-guess) < epsilon) return gx;
            guess = gx;
        }
        return guess;
    }

    int main(void) {
    	printf("%g\n", fixed(0, 0.001, 100000));
        return 0;
    }
```

### Function Pointers
Functions can be passed as arguments to other functions!

So, if we want to pass a function like `double f(double x)` as a parameter, we would use the form `double (*f)(double)`

Eg:
```
	double f(double x) { return x; }
	double eval_func(double x, double (*h)(double)) { return h(x); }
```

We can use this property to generalize `bisect()` to work with any function:

```
	double f0(double x) { return x*x-3; }
    double f1(double x) { return cos(x) - x; }

    void main () {
    	printf("%g\n", bisect(-10, +10, 0.001, 100000, f0));
        printf("%g\n", bisect(-10, +10, 0.001, 100000, f1));
    }
```

## Topics covered on the midterm
* Basics: Variables, Expressions, Loops, Conditionals
* Ints, Doubles
* Functions (function pointers)
* Arrays
* Math.h
* Algorithms: Euclid's, Horner's, Bisection

## Lecture 15
Oct 16th

### Structures
Structures (structs) are **compound data types**.
They group several named member variables** together in memory**, so that they can all be accessed using just one name (pointer).

A simple example would be a struct that holds the time of day (in 24h time):
```
	struct tod {
    	int hours;
        int minutes;
    };
```

Here is how one would actually use that struct in a program:
```
	void main() {
    	struct tod now = {16,50}; // or {.hours = 16, .minutes = 50};
        struct tod later;

        later.hours = 18;
        later.minutes = 0;

        printf("%d:%d\n", now.hours, now.minutes);        // 16:50
        printf("%d:%02d\n", later.hours, later.minutes);  // 18:00
    }
```

We can also use structs as parameters:
```
	void todPrint(struct tod when) {
    	printf("%02d:%02d\n", when.hours, when.minutes);
    }

    void main() {
    	struct tod later {18,0};
        todPrint(later);
    }
```

Can we return structs? Hell yeah we can!
```
	struct tod todAddTime(struct tod when, int hours, int minutes) {
    	when.minutes += minutes;
        when.hours += hours + when.minutes / 60;

        when.minutes %= 60;
        when.hours %= 24;

        return when;
    }

    void main() {
    	struct tod now = {16, 50};
        now = todAddTime(now, 1, 10);
        todPrint(now);
    }
```

We can simplify declaring structs using **typedefs**:
```
	typedef struct {
    	int hours;
        int minutes;
    } tod;

    void main() {
    	tod now = {16, 50};
        ...
    }
```

### Designated Initializers
Sidenote: When defining a structure, you can specify fields to initialize by name, and any unnamed fields are set to 0.

eg: `tod later = {.minutes = 60}` makes `tod={0,60}`
eg: `int a[6] = {[4]=1, [1]=2}` makes `{0,2,0,0,1,0}`

## Lecture 16
Oct 26th

### Complex Numbers
Before programming with complex numbers, we must first understand what complex numbers actually are.
Complex Numbers are numbers that have the following format:
$$
	x + yi
$$
Where $$$ i=\sqrt{-1} $$$ or  $$$ i^2 = -1 $$$.

As youcan see, complex numbers consist of two components:
- $$$ x $$$ is the **real** component.
- $$$ yi $$$ is the **imaginary** component

Here are the various operations we can do with complex numbers:
- Addition: $$$ (x+yi) + (w + zi) = (x + w) + (y+z)i $$$
- Multiplication: $$$ (x+yi)(w + zi) = (xw - yz) + (xz + yw)i $$$

### Complex Numbers in C89
While C99 added a complex numbers library, C89 does not have one, so programmers had to write their own implementation.
Let's write our own implementation. Why? Because.

#### *cplex.h*
```
	#ifndef CPLEX.H
    #define CPLEX.H

        // Define the Complex Number struct
        typedef struct {
        	double r;
            double i;
        } cplex;

        // Declare functions to work with cplex structs
        cplex cplexConstruct(double r, double i);
        cplex cplexAdd(cplex a, cplex b);
        cplex cplexMult(cplex a, cplex b);
        void cplexPrint(cplex a);

    #endif
```

#### *cplex.c*
```
    #include <stdio.h>

    #include "cplex.h"

    cplex cplexConstruct(double r, double i) {
        return (cplex) {.r = r, .i = i};
    }
    void cplexPrint (cplex a) {
        printf("%g + %gi\n", a.r, a.i);
    }

    cplex cplexAdd(cplex a, cplex b) {
        return (cplex) {
            .r = a.r + b.r,
            .i = a.i + b.i
        };
    }
    cplex cplexMult(cplex a, cplex b) {
        return (cplex) {
            .r = a.r * b.r - a.i * b.i,
            .i = a.r * b.i + a.i * b.r
        };
    }
```

#### *cplexMain.c*
```
	#include "cplex.h"

    int main () {
        cplex a = cplexConstruct(1,2);
        cplex b = cplexConstruct(3,4);

        cplex c = cplexAdd(a,b);

        cplexPrint(c);

        c = cplexMult(a,b);

        cplexPrint(c);

        return 0;
    }
```

Ah, another important note:
`if (a == b)` is **NOT** a valid way to check equality between two structs!

### Complex Numbers in C99+
**TL;DR**: It's better.

#### *cplexMain.c*
```
	#include <complex.h>
    #include <stdio.h>

    void main () {
    	double complex a = 1 + 2*I;
        double complex b = 3 + 4*I;

        double complex c = a + b;

		printf("%g + %gi\n", creal(c), cimag(c));

        c = a * b;

        printf("%g + %gi\n", creal(c), cimag(c));

        return 0;
    }
```

## Lecture 17
Oct 28th

### Pagerank
How does Google figure out what you're looking for?
Well, they use a variety of variables:
* content of webpages (anchor text mainly)
* user behavior
* webpage rank (which is static)

That last one is what we want to focus on.

**Pagerank** is a *static* list of webpages.
It is sorted by how "good" the website is.

The rank of a website, i.e: how "good" it is, is determined by how many other websites link to the webpage (logic being that a good website would be reffered to my many other websites).

How does Google build this Pagerank listing?

They might use the **Random Surfer Model**.
This is how it works:
* pretend you are surfing randomly
* at each step, you either:
	* follow a link (probability $$$ \delta $$$) OR
	* Jump to a random page (probability $$$ 1-\delta $$$)
* The Pagerank of a page Q, R(Q), is the probability that you are on page Q after surfing for a long time.

R(Q) is calculated accoring to the following formula:
$$
	R(Q) = \frac{1-\delta}{N} + \delta\sum\limits_{P \rightarrow Q} \frac{R(P)}{out(P)}
$$
Where: 
* $$$ N $$$ is the Total Number of Pages.
* $$$ \sum\limits_{P \rightarrow Q} $$$ is the sum of all pages pointing to Q
* $$$ R(P) $$$ Pagerank of page linking to Q
* $$$ out(P) $$$ Pagerank of page linked from Q

Let's take an example of a simple web:
* X links to Y and Z, and is linked to by Y
* Y links to X, and is linked to by Y and Z
* Z links to Y, and is linked to by X

$$
	R(X) = \frac{1-\delta}{N} + \delta \frac{R(Y)}{1}\\
    R(Y) = \frac{1-\delta}{N} + \delta (\frac{R(X)}{2} + R(Z))\\
    R(Z) = \frac{1-\delta}{N} + \delta \frac{R(X)}{2}
$$

If $$$ \delta = 80\% = \frac{4}{5} $$$ Thus $$$ \frac{1-\delta}{N} = \frac{1}{15} $$$

We can then solve a system of linear equations to find X, Y, and Z.
But I'm not going to solve those by hand, no, that would be too easy...

Let's use our old friend **fixed-point iteration!**

* Let our **initial guess** be that $$$ x = y = z = 0.333... $$$
	* new $$$ x = \frac{1}{15} +  \frac{4}{5}(y=0.333...) $$$
	* new $$$ y = \frac{1}{15} +  \frac{2}{5}(x=0.333...) + \frac{4}{5}(z=0.333...) $$$
	* new $$$ z = \frac{1}{15} +  \frac{2}{5}(x=0.333...) $$$
* Repeat ad-nauseum intill we get a good answer.

After 20 iterations: $$$ x = 0.383, y = 0.396, z = 0.220 $$$

For the whole web, this would take *thousands* of iterations.

## Lecture 18