---
categories: ["projects"]
date: 2023-12-16T10:59:00+01:00
tags: ["guides"]
title: "Wrapping Swift for Python: Bridging the gap with Pybind11"
show-meta: false
toc: false
---

> __Just write it in C and wrap it in Python... I want to see you struggle__
>
>__[@programmersarealsohuman5909](https://www.youtube.com/watch?v=BgxklT94W0I)__

I recently worked on a project that required seamless integration between a Swift Framework and Python. Options like [PyObjC](https://pyobjc.readthedocs.io/en/latest/index.html) exist for simple integrations, but if the Swift code needs to be bundled up, or if you're working on larger projects, the best option is to "wrap it up".

There's no direct way to make Python bindings for Swift, but we can wrap C/C++, for example with [PyBind11](https://github.com/pybind/pybind11), and expose the Swift functions as C symbols with [`@c_decl`](https://forums.swift.org/t/best-way-to-call-a-swift-function-from-c/9829) (which is stil [undocumented and unofficial](https://forums.swift.org/t/formalizing-cdecl/40677)).

You can find the full project code on [Github](https://github.com/gmcheese/wrap-swift-python)

## Requirements

* Python 3.10: I encountered build errors when using Pybind11 + Python 3.11 (see the issue [here](https://github.com/pybind/pybind11/discussions/4333)), so for now, use an environment with Python 3.10 activated.
* XCode and XCode Command-line Tools (`xcode-select --install`). Needed for compiling the Swift code.

## Exposing a Swift Function

First, let's create a file called `main.swift` with the following contents:

```swift
// The Swift Function
@_cdecl("add")
public func add(a: Int, b: Int) -> Int {
    return a + b
}

```

When compiling this file, we can emit a dynamic library with symbols for invocation with C:

```shell
swiftc main.swift -emit-library
```

This will emit a file called `libmain.dylib`, which we can wrap as a regular C library with Pybind11.

## Wrapping the Dynamic Library with Pybind11

Although there is extensive documentation available, the process can often be cumbersome. I really recommend using a LLM of your choice to generate the bindings.

First, install Pybind11 to your environment:

```shell
pip install pybind11
```

Then, let's add the bindings in a file called `bindings.cpp`

```cpp
#include <pybind11/pybind11.h>

namespace py = pybind11;

// C++ wrapper for the Swift function
extern "C" {
    int add(int a, int b); // Declaration of the Swift function
}

PYBIND11_MODULE(example, m) {
    m.def("add", &add, "A function that adds two numbers");
}
```

## Building the Python Project

To bind everything together, create a `setup.py` file and declare the extension:

```python
from setuptools import setup
from pybind11.setup_helpers import Pybind11Extension

ext_modules = [
    Pybind11Extension(
        "mymodule",
        ["bindings.cpp"],
        extra_compile_args=['-std=c++11'],
        library_dirs=['.'],
        libraries=['main'],
    ),
]

setup(
    name="mymodule",
    ext_modules=ext_modules,
)
```

Note that you also need a `pyproject.toml` with the set requirements if you want it to build correctly with `pip`. If you don't, you will get an `no module named pybind11`[^2] when installing. Put the following in your `pyproject.toml`[^1]:

```toml
[build-system]
requires = ["setuptools>=42", "wheel", "pybind11~=2.6.1"]
build-backend = "setuptools.build_meta"
```

We can now install our little package. This will build the wrapper with the naming `mymodule.cpython-310-darwin.so` and place it in the active directory.

```
pip install -e .
```

Verify that you have all the files needed:

```shell
$ ls
bindings.cpp
main.swift
pyproject.toml
libmain.dylib
mymodule.cpython-310-darwin.so
setup.py
```

# Using the Module

The built library can now be directly imported into Python. Fire up a Python shell in the project directory and validate it with:

```python
>>> import mymodule
>>> mymodule.add(2,2)
4
```

Success! This is the first baby step for wrapping Swift for use in Python. There's so many gotcha's and obstacles ahead, but it will be worth it in the long run!

Find the complete project on [Github](https://github.com/gmcheese/wrap-swift-python).

## Next Steps

Wrapping a single file in Swift is a nice example, but for real use cases, you will want to do it for a whole Framework with dependencies set up in XCode. It's certainly possible, and I might do a write-up on that subject one day. Essentially you can export all public symbols from the Framework using the `_cdecl` method described above, and link the static object output within the `.framework` product to the Pybind11 Extension.

I hope this little write-up can help you save some time in your next project. Good luck (you need it)!

[^1]: ["Pybind11 docs"](https://pybind11.readthedocs.io/en/stable/compiling.html?highlight=setup.py#pep-518-requirements-pip-10-required)
[^2]: [PyBind `module not found` on Github Issues](https://github.com/pybind/python_example/issues/32)