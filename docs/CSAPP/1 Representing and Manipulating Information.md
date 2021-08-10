# 1 Representing and Manipulating Information

integer arithmetic: associative and commutative

floating-point arithmetic: not associative (and commutative) (big number eats small number)



![Screen Shot 2021-08-09 at 1.51.46 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%201.51.46%20PM.png)

- Big endian: lower address contains more significant byte

- Intel, Android, iOS operate in little-endian mode
- bi-endian: can be configured to operate as either little- or big-endian (e.g. ARM microprocessors)

## Integer

### Basis

unsigned integer

signed integer / ***two's-complement***:

![Screen Shot 2021-08-09 at 2.21.40 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%202.21.40%20PM.png)

($x_{w-1}$ represents the sign)

![Screen Shot 2021-08-09 at 2.22.24 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%202.22.24%20PM.png)

![Screen Shot 2021-08-09 at 2.25.06 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%202.25.06%20PM.png)

### Conversion

Read two's-complement number's normal value:

- From right to left, starting with the bit after the 1st "1", inverse the bits → abs value
- If the sign bit is `1`, the result is the value represented by the other bits minus $2^w$

Write two's-complement form of given value:

- Decide the sign bit, and use $x + 2^w$ for the remaining bits

### Comparison

If one of the number is unsigned, then compare the numbers by the unsigned rule. The same rule applies to other operations.

### Expansion

Unsigned expansion is simple.

Signed expansion:

![Screen Shot 2021-08-09 at 2.49.30 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%202.49.30%20PM.png)

![Screen Shot 2021-08-09 at 2.54.32 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%202.54.32%20PM.png)

(can be proved by definition)

### Truncation

Truncation is mod:

![Screen Shot 2021-08-09 at 3.00.55 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%203.00.55%20PM.png)

![Screen Shot 2021-08-09 at 3.00.08 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%203.00.08%20PM.png)

### Arithmetic

#### Addition

![Screen Shot 2021-08-09 at 3.04.11 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%203.04.11%20PM.png)

![Screen Shot 2021-08-09 at 3.04.37 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%203.04.37%20PM.png)

![Screen Shot 2021-08-09 at 3.07.13 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%203.07.13%20PM.png)

Add the two's-complement form of the numbers directly, and illustrate the result as a two's-complement number.

![Screen Shot 2021-08-09 at 3.17.20 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%203.17.20%20PM.png)

![Screen Shot 2021-08-09 at 3.17.43 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%203.17.43%20PM.png)

#### Negation

![Screen Shot 2021-08-09 at 10.48.19 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%2010.48.19%20PM.png)

At bit-level,

```c
int8_t x = 12;	// 0000 1100
x = -x;					// 1111 0100

```

![Screen Shot 2021-08-09 at 10.55.35 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%2010.55.35%20PM.png)

At bit-level, `-x == ~x + 1`:

```c
uint8_t x = 12;	// 0000 1100
x = -x;					// 1111 0100

uint8_t x = -12;	// 1111 0100
x = -x;						// 0000 1100
```

#### Multiplication

![Screen Shot 2021-08-10 at 11.40.39 AM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-10%20at%2011.40.39%20AM.png)

*Given that integer multiplication is more costly than shifting and adding, many C compilers try to remove many cases where an integer is being multiplied by a constant with combinations of shifting, adding, and subtracting.*

For example, suppose a program contains the expression x*14. Recognizing that 14 = 23 + 22 + 21, the compiler can rewrite the multiplication as (x<<3) + (x<<2) + (x<<1) .

#### Division

!!! danger "Rounding to zero and rounding up/down"
    

    For Python,
    
    ```python
    >>> -13/3
    -4.333333333333333
    >>> -13//3
    -5
    ```
    
    (It's rounding down.)
    
    For C,
    ```c
    (lldb) print -13/3.0
    (double) $1 = -4.333333333333333
    (lldb) print -13/3
    (int) $2 = -4
    (lldb)
    ```
    (It's rounding to zero.)
    
    So two’s-complement division by a power of 2 in C means the result should be rounded to zero, leading to the difference with simply shifting right.
    
    ![Screen Shot 2021-08-09 at 3.48.51 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%203.48.51%20PM.png)
    
    ![Screen Shot 2021-08-09 at 4.14.16 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-09%20at%204.14.16%20PM.png)

![image-20210809161448105](1%20Representing%20and%20Manipulating%20Information.assets/image-20210809161448105.png)



## Floating Point

