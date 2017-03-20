```python

# In Python...

class JS_Object:

    def __init__(self, prototype = None):       # When called with prototype, equivalent to Object.create()

        if prototype is not None:
            if type(prototype) is not JS_Object:
                raise TypeError

        object.__setattr__(self, "dict", {})
        object.__setattr__(self, "prototype", prototype)

    def __getattribute__(self, key):

        key = str(key)

        try:
            d = object.__getattribute__(self, "dict")
            return d[key]
        except KeyError:
            prototype = object.__getattribute__(self, "prototype")
            if prototype == None:
                return None                     # Strictly, should return some "undefined" object
            return prototype[key]

    def __setattr__(self, key, val):

        key = str(key)

        d = object.__getattribute__(self, "dict")
        d[key] = val

    def __getitem__(self, key):

        return object.__getattribute__(self, "__getattribute__")(key)

    def __setitem__(self, key, val):

        return object.__getattribute__(self, "__setattr__")(key, val)


if __name__ == "__main__":
    foo = JS_Object()
    foo.a = 1
    foo["b"] = 2
    foo.c = 3
    foo["d"] = 4

    bar = JS_Object(foo)
    bar.c = 300

    print(foo.a)
    print(foo.b)
    print(foo["c"])
    print(foo["d"])

    print(bar.a)
    print(bar.b)
    print(bar["c"])
    print(bar["d"])
```
