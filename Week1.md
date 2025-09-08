### Beginner’s Primer

1. Module & Function

Function = a block of code that does something specific.

def add(a, b):
    return a + b

You call it with add(2,3) → 5.

Module = a Python file that contains functions, classes, or variables.
Example: core/alloc.py is a module with your rebalancing math.
You can import from it:

from core.alloc import compute_value_and_weights

👉 Functions live inside modules. Modules live inside your project.