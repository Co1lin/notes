# 1 Representing and Manipulating Information

integer arithmetic: associative and commutative

floating-point arithmetic: not associative (but also commutative) (big number eats small number)



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

!!! danger "Shifting right of negative two's-complement"
    
    ```
    1001 0000 >> 1 =
    1100 1000
    ```
    Keep the sign bit, and padding 1 to the left of the remaining bits.

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

### IEEE Floating-Point Representation

![image-20210810120020515](1%20Representing%20and%20Manipulating%20Information.assets/image-20210810120020515.png)

**CAUTION: $f = 0.f_{n-1} \dots f_0 = {f_{n-1} \dots f_0 \over 2^n}$**

![image-20210810120349142](1%20Representing%20and%20Manipulating%20Information.assets/image-20210810120349142.png)

(NaN: not a number; e.g. $\sqrt{-1}$, $\infty - \infty$)

#### Floating Point Representation → Value

```
0 1110 101
s  e    f

Bias = 2^(4-1) - 1 = 7
E = e - 7 = 14 - 7 = 7
e != 0 => M = f + 1 = 5/2^3 + 1 = 13/8
Value = (-1)^0 * M * 2^E = 13/8 * 2^7 = 13*16 = 208
```

#### Value → Floating Point Representation

```
129
10000001

1.0000001 * 2^7
0 (e,E=e-Bias) (f,M=f+1) = M * 2^E
Bias = 2^(4-1) - 1 = 7
e = E+Bias = 7+7 = 14 -> 1110(bin)
f = M-1 = 0.0000001

0 1110 000
```

### Rounding

![Screen Shot 2021-08-10 at 2.36.23 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-10%20at%202.36.23%20PM.png)

Round-to-**even** is also called round-to-nearest .

```
Round to quater:
10.11100 -> 11.00
10.10100 -> 10.10
```

### Operations

No associative property!

Satisfies:

1. closure of addition or multiplication: 
    - although 0 * inf yields *NaN*
2. commutative: x\*y = y\*x, x+y = y+x
3. monotonicity: 
    - if a≥b ,then x+a ≥ x+b for any values of a, b, and x other than *NaN*
    - ![Screen Shot 2021-08-10 at 2.54.45 PM](1%20Representing%20and%20Manipulating%20Information.assets/Screen%20Shot%202021-08-10%20at%202.54.45%20PM.png)





