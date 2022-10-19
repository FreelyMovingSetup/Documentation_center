!!! info "Purpose"
    This class is the base class for any module. It principally comprises attributes and methods to access modules by name from json commands (see `Maze API` section of the documentation). It also allows basic function that can be usefull and common to any module, sensor or actuator alike, such as deactivation / reactivation of the module. 

!!! warning
    This class is not meant to be used alone, but to de derived to actually do something usefull.

## Example Usage :


```c++
modulex module_object = modulex("MyModuleName")
```

## Constructor :

```c++
modulex module_object;
```
