# memoization using lru-cache

If you are repeatedly using a function that takes time to produce a result
or calls a remote API it is a good idea to cache the result. You can do this using 
a decorator: `lru-cache`.

As long as we are on the subject of decorators - I came across this [excellent decorator](https://stackoverflow.com/questions/1622943/timeit-versus-timing-decorator) for timing
python functions:

```python
from functools import wraps
from time import time

def timing(f):
    @wraps(f)
    def wrap(*args, **kw):
        ts = time()
        result = f(*args, **kw)
        te = time()
        print 'func:%r args:[%r, %r] took: %2.4f sec' % \
          (f.__name__, args, kw, te-ts)
        return result
    return wrap
```

We can use it to time how long the function takes with the `lru_cache` decorator.
Note that the `lru_cache` decorator is added after the `@timing` decorator. If you have it the other way around
the cache returns the result immediately and no part of the timing function is used - so you only get the previous result.

```python
from functools import lru_cache

@timing
@lru_cache()
def costly_function(x):
    return sum(list(map(lambda x: x^2, range(pow(10, x)))))


# First run
costly_function(7)
# func:'costly_function' args:[(7,), {}] took: 1.9150 sec

# Second run
costly_function(7)
func:'costly_function' args:[(7,), {}] took: 0.0000 sec

```

This method of caching functions based on their input arguments is also known as __[memoization](https://en.wikipedia.org/wiki/Memoization)__. 
