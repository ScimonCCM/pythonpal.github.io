---
sort: 1
---

# Running Python in OpenFOAM with pybind11

In 2022, Rodriguez and Cardiff introduced an approach to run Python codes in OpenFOAM ([see paper](https://tinyurl.com/pybind11foam)), based on the header-only library pybind11.

With pybind11, they were able to embed a Python interpreter in OpenFOAM and interact with it. The conceptual flowchart they presented is shown in the figure.

<img src="/images/6steps.png" alt="6 steps to communicate Python and OpenFOAM">


To achieve an efficient data transfer, they combined [_ctypes_](https://docs.python.org/3/library/ctypes.html), NumPy's built-in support for [_ctypes_](https://docs.python.org/3/library/ctypes.html) and OpenFOAM's `data` (or `cdata`) functions for OpenFOAM `Lists/fields`. 


**pythonPal hides the details related to `ctypes`, `NumPy` and `data` by offering higher-level methods to transfer data between OpenFOAM and Python and execute general Python code.**