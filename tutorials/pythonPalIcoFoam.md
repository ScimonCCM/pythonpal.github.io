---
sort: 1
---

# My first tutorial: pythonPalIcoFoam

You can watch a short videotutorial for this case, [here](https://tinyurl.com/pythonPalTutorial).

In this tutorial, we create a modified version of `icoFoam` called `pythonPalIcoFoam`, where we use pythonPal to calculate the specific kinetic energy, $$k = \frac {1}{2} U \cdot U $$

Provided that _pybind11_ is installed (see [installing](/preliminaries/installingPybind.html#installing-pybind11)), the steps to go from `icoFoam` to `pythonPalIcoFoam` are:

- Copy the icoFoam directory to a directory pythonPalIcoFoam, then rename the .C file inside it and the C file and executable file names inside Make/files ( Just replace "icoFoam" with "pythonPalIcoFoam").
<!-- - Copy the base `icoFoam` solver to your working directory and rename it to `pythonPalIcoFoam.C`. -->

- Copy the `pythonPal.H` file to the pythonPalIcoFoam directory.

- Include `pythonPal.H` at the top of `pythonPalIcoFoam.C`;

```C
#include "pythonPal.H"
```

- In `createFields.H`, construct a `volScalarField` called `k`, initialised to zero;

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

- In the parent folder, create a Python file and define a function that receives the `U` field from OpenFOAM and calculates `k`. We called that file `python_script.py` and the function `calculatek`.

```Python
def calculatek(field):
  return np.sum(field * field, axis = 1)[:, np.newaxis] / 2
```

- In `pythonPalIcoFoam.C`, create an object of type pythonPal and load the Python file `python_script.py`:

```bash
pythonPal myPythonPal("python_script.py", true);
```

- At the end of the time-loop, pass the `U` and `k` fields to Python via pythonPal:

```C
myPythonPal.passToPython(U, "U");
myPythonPal.passToPython(k, "k");
```

- Calculate `k` via pythonPal:

```C
myPythonPal.execute("k[:, :] = calculatek(U)");
```

- Finally, write the `k` field to disk, e.g. for viewing in ParaView.

```C
k.write();
```

Figure shows the cavity case’s velocity magnitude and specific kinetic energy field results.

<img src="/images/pythonPalResult.PNG" alt="cavity case's results" width="80%" >

<!-- You can watch a short videotutorial for this case, [here](https://tinyurl.com/pythonPalTutorial). -->

To learn more about how pythonPal works, see [How Does It Work?](../howDoesItWork/runningPythoninOpenFOAMwithpybind11.md).