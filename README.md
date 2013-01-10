# nose-test-sets

## Installation

```$ pip install nose-test-sets```

## Usage

To define your set of common tests, create a `TestSetBuilder`, and use
`add_test` to add tests. Each test should accept the same name of arguments.
Say you define some tests for an adder in the module `common_tests`:

```python
from nose_test_sets import TestSetBuilder

test_set_builder = TestSetBuilder()

@test_set_builder.add_test
def adding_zero_to_zero_returns_zero(adder):
    assert adder.add(0, 0) == 0
    
create = test_set_builder.create
    
```

To run the tests against a specific implementation, you create a set of tests
using the `create` function that we defined above:

```python
import contextlib

import adder_tests

@contextlib.contextmanager
def create_adder():
    return StandardAdder()
    
StandardAdder = adder_tests.create("StandardAdder", create_adder)
```

The first argument to `create` should be the name of the concrete test set.
Any further arguments should be context managers that supply the arguments
that will be used by each test.
