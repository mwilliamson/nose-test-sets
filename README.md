# nose-test-sets

## Installation

```$ pip install nose-test-sets```

## Usage

To define your set of common tests, create a `TestSetBuilder`, and use
`add_test` to add tests. Each test should accept the same name of arguments.
Say you define some tests for an adder in the module `adder_test_set`:

```python
from nose_test_sets import TestSetBuilder

test_set_builder = TestSetBuilder()
test = test_set_builder.add_test

@test
def adding_zero_to_zero_returns_zero(adder):
    assert adder.add(0, 0) == 0
    
@test
def adding_one_to_two_returns_three(adder):
    assert adder.add(1, 2) == 3
    
create = test_set_builder.create
```

To run the tests against a specific implementation, you create a set of tests
using the `create` function that we defined above:

```python
import adder_test_set

def _run_test_with_standard_adder(test_func):
    adder = StandardAdder()
    return test_func(adder)
    
StandardAdderTests = adder_test_set.create(
    "StandardAdderTests",
    _run_test_with_standard_adder
)
```

The first argument to `create` should be the name of the concrete test set.
The second argument is a function that can run each of the test functions.
In the example above, to run the tests in `StandardAdderTests`, nose-test-sets
ends up calling
`_run_test_with_standard_adder(adding_zero_to_zero_returns_zero)` and
`_run_test_with_standard_adder(adding_one_to_two_returns_three)`.

