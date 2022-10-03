---
# sort: 1
---

# API

## Class constructor

>pythonPal (const fileName& pythonScript, const bool& debug = true)

The python file address (`pythonScript`) is a required argument. An optional `debug` switch can be given to control how much information is printed to the standard output 

## Public member functions: 

> void passToPython(List& myList, const py::str& fieldNameInPython) const 

This function creates a NumPy array in the Python interpreter with the name `fieldNameInPython` which references the data from the OpenFOAM `List/Field` `myList` . After this, any transformation of the `fieldNameInPython` or `myList` will be propagated to the other side.
    
When an OpenFOAM `GeometricField` is passed via `passToPython`, it will only transfer the internal field; if required, the boundary patches must be independently transferred.


> void execute(const word& command) const 

This executes the Python command given by `command`.
