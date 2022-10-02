---
# sort: 1
---

# API

## Class constructor

>pythonPal (const fileName& pythonScript, const bool& debug = true)

The python file address (_pythonScript_) is a required argument. An optional _debug_ switch can be given to control how much information is printed to the standard output 

## Public member functions: 

> void passToPython(List& myList, const py::str& fieldNameInPython) const 

This function creates a NumPy array in the Python interpreter with the name _fieldNameInPython_ which references the data from the OpenFOAM _List/Field_ _myList_ . After this, any transformation of the _fieldNameInPython_ or _myList_ will be propagated to the other side.
    
When an OpenFOAM _GeometricField_ is passed via _passToPython_, it will only transfer the internal field; if required, the boundary patches must be independently transferred.


> void execute(const word& command) const 

This executes the Python command given by _command_.
