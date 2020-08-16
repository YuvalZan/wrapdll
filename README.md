# wrapdll
Allows using python annotaions to wrap dlls using ctypes.

Usefull for when you have to use a dll in your python project and you want to make the dll function more pythonic.



### Example
The best way to understand this package is with an example.
In this example we wrap one of the exported functions of `python3.dll` from the installation directory
```
import sys
from ctypes import *
from pathlib import Path
from wrapdll import BaseDllWrapper, wrapdll 

class PythonAPI(BaseDllWrapper):
    """
    A wrapper to ctypes.pythonapi
    """

    def __init__(self):
        api = str(Path(sys.exec_prefix, 'python3.dll'))
        super().__init__(api)

    @wrapdll
    def PyBytes_FromStringAndSize(self, v: c_char_p, len: c_size_t) -> py_object:
        """
        Return a new bytes object with a copy of the string v as value and length len on success, and NULL on failure.
        If v is NULL, the contents of the bytes object are uninitialized.
        """
        pass

```
The args and return type are automatically set from the annotations.
In addition, the function gets a better signature and doc when you run `help` on it:
```
In [2]: api = PythonAPI()

In [3]: help(api.PyBytes_FromStringAndSize)
Help on method PyBytes_FromStringAndSize in module __main__:

PyBytes_FromStringAndSize(v: ctypes.c_char_p, len: ctypes.c_ulong) -> ctypes.py_object method of __main__.PythonAPI instance
    Return a new bytes object with a copy of the string v as value and length len on success, and NULL on failure.
    If v is NULL, the contents of the bytes object are uninitialized.
```

### Features
* BaseDllWrapper class and wrapdll decorator to create wraped dll classes
* errcheck, allowed_values and forbidden_values decorators to automatically raise an exception on invalid values. (Look at [tests\wraped_kernel32.py](wrapdll/tests/wraped_kernel32.py) for example)
* RCEnum to create IntEnums with a built-in error message. 
  Works magic when used as a return type in conjuncture with the allowed/forbidden decorators to automatically raise a comprehensive exception.

### Installation
```
pip install wrapdll
```

