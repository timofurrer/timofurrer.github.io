---
title: "Python Pass vs. Ellipsis"
date: 2021-11-05T09:00:12+01:00
draft: false
categories: [ "python", "software engineering" ]
---

I've recently had a nerdy discussion with a colleague during a code review
about when to use
Pythons `pass` statement[^1] vs. the ellipsis literal `...`[^2]

You've probably seen the `pass` statement in Python code as a
means to indicate that a block is intentionally left empty
to avoid a `SyntaxError`.
Like in the following `except` block where we expect that
an exception might be raised but just want to ignore it:

```python
try:
    client.get(id=42)
except ApiException:
    pass
```

Somehow the ellipsis literal `...` has gotten some hype lately and
some people started doing this:

```python
try:
    client.get(id=42)
except ApiException:
    ...
```

While this is syntactically valid Python code the semantics are
weird to me.

To me, the `pass` statement effectively communicates that
we should just *pass* this block without running any code at all.
While the `...` are more like
*“there is something to be expected here in the future”*.
Therefore, the `...` can be thought of a placeholder.

Whenever I see the `...` my head auto-plays a [*Dun Dun Dun* sound effect](https://youtu.be/Bnmp_oAHRC0) -
to emphasize the suspension of code :speak_no_evil:.
It would be a pity if it's a suspension of code until all eternity if you use at as a `pass` replacement.

### Where should I use the `...` ?

As I've already mentioned you should use the `...` as a placeholder where eventually
some could should appear, but you don't necessarily want to `raise NotImplementedError`.

A similar situation where I use it is with Pythons [Abstract Base Classes](https://docs.python.org/3/library/abc.html)
to indicate that the function body needs to be implemented by a subclass.

```python
from abc import abstractmethod

class AbstractEventLoop:
    @abstractmethod
    def run(self, ...):
        ...

```

The next place where I use it is in Python [stub files](https://www.python.org/dev/peps/pep-0484/#stub-files)
to indicate an *ignored* function body:

```python
def get(id: int) -> Model: ...
```

The fourth place is in extended slicing with custom container types, for what the `...` was originally introduced for.
For example, the [numpy](https://numpy.org/doc/stable/reference/arrays.indexing.html) array indexing supports it:

```python
from numpy import arange
a = arange(16).reshape(2, 2, 2, 2)

flat_a = a[..., 0].flatten()
```

### But what is the `...` literal ?

The ellipsis literal `...` is syntactic sugar for the `Ellipsis` object,
which is the sole instance of the `types.EllipsisType` type.
You can use it like every other object in Python, except that you can't
create a new one. Thus, when you override it (yes, that's possible: `__builtins__.Ellipsis = 42`)
you can't really get it back :shrug: - thus, don't do it.

[^1]: [pass statement Python docs](https://docs.python.org/3/tutorial/controlflow.html#pass-statements)

[^2]: [Ellipsis Python docs](https://docs.python.org/3/library/constants.html#Ellipsis)