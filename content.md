In this page, we will compare the performance of the `sinc` function implemented in `scipy.special` with a custom Python implementation using the `math` module. We will compare the performance for both a single value, and for a larger sequence.

# Scalar Sinc Comparison

In this comparison we will calculate the sinc of a single number. This will be repeated a large number of times to improve the accuracy of the measurement. 

## Native Python Implementation

The code below defines a custom Python implementation of the sinc function and times its performance when it is called for a single scalar value.

```py-cell
import time
import math

# Define a sinc function using native Python
def sinc_non_scipy(x):
  return math.sin(math.pi * x)/(math.pi * x)

repetitions = 100000

start_time = time.time()
for i in range(repetitions):
  c = sinc_non_scipy(1)
print('Non-SciPy single sinc:', time.time() - start_time)
```

## SciPy Implementation

The code below uses `scipy.special.sinc()` to calculate the sinc function for a single scalar value.

```py-cell
import time
from scipy.special import sinc
import numpy as np

repetitions = 100000

start_time = time.time()
for i in range(repetitions):
  c = sinc(1)
print('SciPy single sinc:', time.time() - start_time)
```

## Explanation

For a single scalar value, the hand-written implementation using Python's `math` module is typically faster than `scipy.special.sinc()`. This is because the overhead of calling a compiled function in SciPy outweighs the simple arithmetic operations performed in the custom implementation.

# Array Sinc Comparison

In this comparison we will calculate the sinc function for a large sequence of values, both using a custom Python implementation and `scipy.special.sinc()`. This will demonstrate the performance difference when working with arrays.

## Native Python Implementation Using Loops

The code below times the performance of calculating the sinc function of a list of values using a custom Python implementation, applied naively using a loop and build a list using its `append` method:

```py-cell
import time
import math

def sinc_non_scipy(x):
  return math.sin(math.pi * x)/(math.pi * x)

# Create the list we will take the sinc of
a = list(range(1, 1000000))

start_time = time.time()
b = []
for entry in a:
  b.append(sinc_non_scipy(entry))
print('Loop-based sinc of list:', time.time()- start_time)
```

## Native Python Implementation Using `map`

The code below times the performance of calculating the sinc function of a list of values using a custom Python implementation, applied using the `map` function. This is more advanced code, so don't worry if it looks unfamiliar. It has been chosen to be the most efficient way to apply a function to each element of a list in pure Python.

```py-cell
import time
import math

def sinc_non_scipy(x):
  return math.sin(math.pi * x)/(math.pi * x)

# Create the list we will take the sinc of
a = list(range(1, 1000000))

start_time = time.time()
c = list(map(sinc_non_scipy, a))
print('Map-based sinc of list:', time.time() - start_time)
```

## SciPy Implementation

The code below uses the `scipy.special.sinc()` function on a NumPy array.

```py-cell
import time
import math
from scipy.special import sinc
import numpy as np

# Create the array we will take the sinc of
a = np.arange(1, 1000000)

start_time = time.time()
c = sinc(a)
print('SciPy sinc of array:', time.time() - start_time)
```

## Explanation

The slowest approach is using a loop to apply the custom Python sinc function to each element of the list as Python must explicitly execute each iteration in the interpreter.

Using a `map` function is faster than a loop because it avoids the explicit Python iteration and applies the function to each element more efficiently.

The fastest approach is using `scipy.special.sinc()` on a NumPy array, as it leverages highly optimised compiled code and vectorised operations.

# Choosing the Right Implementation

In most cases, using the SciPy implementation with a NumPy array is the preferred approach. It is clearer and more readable, and requires the smallest amount of code to be written, maintained and tested. Even if being applied to a single value, the performance costs of an individual calculation are small as the overall operation is not expensive, and so the convenience and readability benefits outweigh the minor performance considerations.