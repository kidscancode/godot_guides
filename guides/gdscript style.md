# GDScript Style Guide

### Description
This document describes a set of conventions for GDScript code style. The goal is to encourage writing clean, readable code and promote consistency across projects, discussions, and tutorials. Since GDScript is heavily influenced by Python, this guide is inspired by and takes many suggestions from [PEP 8](https://www.python.org/dev/peps/pep-0008/).

Hopefully, as the Godot community grows, this will also encourage development of auto-formatting tools.

_Note:_ Many of these guidelines are encouraged by Godot's built-in script editor's default settings. Let it help you.

### Code Structure

#### Indentation
Indent type: Tabs _(editor default)_
Indent size: 4 _(editor default)_

Each indentation level should be one greater than the block containing it.

**Good**
```
for i in range(10):
    print("hello")
```

**Bad**
```
for i in range(10):
        print("hello")
```

Continuation of long lines should use at least two indent levels to distinguish it from a regular code block.

**Good**
```
effect.interpolate_property(sprite, 'transform/scale',
            sprite.get_scale(), Vector2(2.0, 2.0), 0.3,
            Tween.TRANS_QUAD, Tween.EASE_OUT)
```

**Bad**
```
effect.interpolate_property(sprite, 'transform/scale',
    sprite.get_scale(), Vector2(2.0, 2.0), 0.3,
    Tween.TRANS_QUAD, Tween.EASE_OUT)
```

#### Blank lines

Function and class definitions should be surrounded by a blank line.

Use blank lines where appropriate inside functions to separate logical sections.

#### One Statement per line

Never combine multiple statements on a single line. No, C programmers, not even when you have a single line conditional statement!

**Good**
```
if position.x > width:
    position.x = 0

if flag:
    print("flagged")
```

**Bad**
```
if position.x > width: position.x = 0

if flag: print("flagged")
```

#### Avoid Unnecessary Parentheses

Avoid using parentheses in expressions and conditional statements. Unless necessary for order of operations, they add nothing and reduce readability.

**Good**
```
if is_colliding():
    queue_free()
````
**Bad**
```
if (is_colliding()):
    queue_free()
````


#### Whitespace

One space should always be used around operators and after commas. Avoid extra spaces in dictionary references and function calls.

**Good**
```
position.x = 5
position.y = mpos.y + 10
dict['key'] = 5
myarray = [4, 5, 6]
print('foo')
```

**Bad**
```
position.x=5
position.y = mpos.y+10
dict ['key'] = 5
myarray = [4,5,6]
print ('foo')
```

**Never!**
```
x        = 100
y        = 100
velocity = 500
```

**Exceptions**
```
# When using multiple operators in an expression
c = a*a + b*b     # good
c = a * a + b * b # bad
```

### Other

### Naming Conventions
These naming conventions follow the Godot Engine style. Breaking these will make your code clash with the built-in naming conventions, which is ugly.

#### Classes and Nodes
Class names use CapWords: `KinematicBody`

#### Functions and Variables
Function and variable names use snake_case: `get_node()`

Private functions and variables may be designated by a single leading underscore: `_ready()`

#### Constants
Constants should generally be named using ALL_CAPS: `MAX_SPEED`
