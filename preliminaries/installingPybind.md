---
# sort: 1
---

# Getting set up

## Installing pybind11

pythonPal uses the header-only library [pybind11](https://pybind11.readthedocs.io/en/stable/index.html) to enable the Python/OpenFOAM communication. 
<!-- For more details, see [How Does It Work?](../howDoesItWork/runningPythoninOpenFOAMwithpybind11.md). -->

We recommend installing pybind11 with [Conda](https://docs.conda.io/en/latest/). On a *nix system:

```bash
conda install −c conda−forge pybind11
```

## Setting up the environment variables

To use the pythonPal, we need to let OpenFOAM know where both pybind11 and the Python interpreter are located in the system. 

An easy way to do this is to declare two environment variables, `PYBIND11_INC_DIR` and `PYBIND11_LIB_DIR`, whose values are retrieved with the following commands:

```bash
export PYBIND11_INC_DIR=$(python3 −m pybind11 −−includes)
export PYBIND11_LIB_DIR=$(python −c ’from distutils import sysconfig; print(sysconfig.get_config_var("LIBDIR"))’)
```

Then, this environment variables must be included in your OpenFOAM code:

- Include `PYBIND11_INC_DIR` in the `EXE_INC` field, for example:

```bash
EXE_INC = \
 $(PYBIND11 INC DIR)
```

- Include `PYBIND11_LIB_DIR` and the Python C dynamic library in the `EXE_LIBS` field (or in the `LIB_LIBS` field if compiling a library), for example:

```bash
EXE_LIBS = \
 −L$(PYBIND11 LIB DIR) \
 −lpython3.8
```

## Download pythonPal

Finally, download the [pythonPal.H](pythonPal.H) file and include it in your OpenFOAM application, as explained in the [tutorials](../tutorials/pythonPalIcoFoam.md).