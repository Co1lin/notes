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

## Decorator

### Function

Naive style:

```python
# a function (as a decorator) which returns a function
def a_new_decorator(a_func):
 
    def wrapTheFunction():
        print("I am doing some boring work before executing a_func()")
 
        a_func()
 
        print("I am doing some boring work after executing a_func()")
 
    return wrapTheFunction

# a function decorated by the decorator
@a_new_decorator
def a_function_requiring_decoration():
    """Hey you! Decorate me!"""
    print("I am the function which needs some decoration to "
          "remove my foul smell")
    
# '@' style in function definition is a syntactic sugar of:
def a_function_requiring_decoration():
    """Hey you! Decorate me!"""
    print("I am the function which needs some decoration to "
          "remove my foul smell")
a_function_requiring_decoration = a_new_decorator(a_function_requiring_decoration)

# call the function as normal
```

To avoid that the name of the function is replaced by its decorator:

```python
from functools import wraps

# decorator def.
def a_new_decorator(a_func):
    @wraps(a_func)
    def wrapTheFunction():
        print("I am doing some boring work before executing a_func()")
        a_func()
        print("I am doing some boring work after executing a_func()")
    
    return wrapTheFunction

# decorate a function
@a_new_decorator
def a_function_requiring_decoration():
    """Hey yo! Decorate me!"""
    print("I am the function which needs some decoration to "
          "remove my foul smell")
```

### Class

```python
from functools import wraps
 
class logit(object):
    def __init__(self, logfile='out.log'):
        self.logfile = logfile
 
    def __call__(self, func):
        @wraps(func)
        def wrapped_function(*args, **kwargs):
            log_string = func.__name__ + " was called"
            print(log_string)
            # 打开logfile并写入
            with open(self.logfile, 'a') as opened_file:
                # 现在将日志打到指定的文件
                opened_file.write(log_string + '\n')
            # 现在，发送一个通知
            self.notify()
            return func(*args, **kwargs)
        return wrapped_function
 
    def notify(self):
        # logit只打日志，不做别的
        pass

@logit()
def myfunc1():
    pass
```

