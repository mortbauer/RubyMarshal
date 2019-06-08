RubyMarshal
===========

Read and write Ruby-marshalled data.
Only basics Ruby data types can be read and written: 

  * `float`,
  * `bool`,
  * `int`,
  * `str` (mapped to `rubymarshal.classes.RubyString` if dumped with instance variables),
  * `nil` (mapped to `None` in Python),
  * `array` (mapped to `list`),
  * `hash` (mapped to `dict`),
  * symbols and other classes are mapped to specific Python classes.

Installation
------------

    pip install rubymarshal

Usage
-----

```python3
    from rubymarshal.reader import loads, load
    from rubymarshal.writer import writes, write
    with open('my_file', 'rb') as fd:
        content = load(fd)
    with open('my_file', 'wb') as fd:
        write(fd, my_object)
    loads(b"\x04\bi\xfe\x00\xff")
    writes(-256)
```

You can map Ruby types to Python ones:

```python3
    from rubymarshal.reader import loads
    from rubymarshal.classes import RubyObject

    class DomainError(RubyObject):
        ruby_class_name = "Math::DomainError"
    
    class_mapping = {"Math::DomainError": DomainError}

    loads(b'\x04\x08c\x16Math::DomainError', class_mapping=class_mapping)
```

You can use Ruby's symbols:

```python3
    from rubymarshal.reader import loads
    from rubymarshal.writer import writes
    from rubymarshal.classes import Symbol
    
    x = Symbol("test")
    dump = writes(Symbol("test"))
    y = loads(dump)
    assert y is x

```
  
Infos
-----

Code is on github: https://github.com/d9pouces/RubyMarshal 
Documentation is on readthedocs: http://rubymarshal.readthedocs.org/en/latest/ 
Tests are on travis-ci: https://travis-ci.org/d9pouces/RubyMarshal