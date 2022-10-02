---
sort: 1
---

# My first tutorial: pythonPalIcoFoam


In this tutorial, we create a modified version of _icoFoam_ called _pythonPalIcoFoam_, where we use pythonPal to calculate the specific kinetic energy, _k=U*U/2_, via Python. Then, we solve the classic [cavity](https://doc.cfd.direct/openfoam/user-guide-v6/cavity) case with this.

Provided that _pybind11_ is installed (see [installing](/preliminaries/installingPybind.html#installing-pybind11)), the steps to go from _icoFoam_ to _pythonPalIcoFoam_ are:

- Copy the base _icoFoam_ solver to your working directory and rename it to _pythonPalIcoFoam.C_.

- Copy the _pythonPal.H_ file to your working directory.

- Include _pythonPal.H_ at the top of _pythonPalIcoFoam.C_;

```C
#include "pythonPal.H"
```

- In _createFields.H_, construct a _volScalarField_ called _k_, initialised to zero;

```C
volScalarField k
(
    IOobject
    (
        "k",
        mesh.time().timeName(),
        mesh,
        IOobject::READ_IF_PRESENT,
        IOobject::AUTO_WRITE
    ),
    mesh,
    dimensionedScalar("k", dimensionSet(0, 2, -2, 0, 0, 0, 0), 0.0)
);
```

- In the parent folder, create a Python file and define a function that receives the _U_ field from OpenFOAM and calculates _k_. 

We called that file _python_script.py_ and the function _calculatek_.

```bash
touch python_script.py
```

```Python
def calculatek(field):
  return np.sum(field * field, axis = 1)[:, np.newaxis] / 2
```

- In _pythonPalIcoFoam.C_, create an object of type pythonPal and load the Python file _python_script.py_:

```bash
pythonPal myPythonPal("python_script.py", true);
```

- At the end of the time-loop, pass the _U_ and _k_ fields to Python via pythonPal:

```C
myPythonPal.passToPython(U, "U");
myPythonPal.passToPython(k, "k");
```

- Calculate _k_ via pythonPal:

```C
myPythonPal.execute("k[:, :] = calculatek(U)");
```

- Finally, write the _k_ field to disk, e.g. for viewing in ParaView.

```C
k.write();
```

Figure shows the cavity case’s velocity magnitude and specific kinetic energy field results.

<img src="/images/pythonPalResult.PNG" alt="cavity case's results" width="80%" >