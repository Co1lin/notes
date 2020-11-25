# Python

## gmpy2

### is_prime

By Miller-Rabin tests.

```python
import gmpy2
gmpy2.is_prime(x)
```

## Sys.argv

Example:

```python
scale = 10000
step = 10000
loop = 10
if len(sys.argv) >= 2:
    scale = int(sys.argv[1])
    if len(sys.argv) >= 3:
        step = int(sys.argv[2])
        if len(sys.argv) >= 4:
            loop = int(sys.argv[3])
```

## OS

Execute a command and get its return value:

```python
not_same = os.system('diff {} {}'.format('ans.txt', 'output.txt'))
```

Execute a command and get its output as **list** by **popen** (seperated by lines) (pay attention to the stream redirection):

```python
std_info = os.popen(
    '(gtime -f"%e\t\t%M\t\t" ./std < {} > {}) 2>&1'.format('input.txt', 'ans.txt')
).readlines()
```

## Format

Example:

```python
not_same = os.system('diff {} {}'.format('ans.txt', 'output.txt'))
```

The parameters substitute `{}` respectively in the original string.