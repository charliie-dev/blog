# Python | Which Dataclass to Use

Python has many ways to bundle a few fields together. These notes follow mCoding's comparison of
them:[^mcoding] a feature matrix first, then each option with an example, then speed and memory,
then the verdict. Every example stores the same three fields: an `int`, a `float` and a `str`.

## Feature matrix

| Container         | Mutable | Immutable | Slots | Defaults | Default factory | Access by keyword | Converters | Validators | Type-safe | Standard library |
| ----------------- | ------- | --------- | ----- | -------- | --------------- | ----------------- | ---------- | ---------- | --------- | ---------------- |
| `tuple`           | ✗       | ✓         | ✓     | ✗        | ✗               | ✗                 | ✗          | ✗          | ✗         | ✓                |
| `namedtuple`      | ✗       | ✓         | ✓     | ✓        | ✗               | ✓                 | ✗          | ✗          | ✗         | ✓                |
| `NamedTuple`      | ✗       | ✓         | ✓     | ✓        | ✗               | ✓                 | ✗          | ✗          | ✓         | ✓                |
| `dict`            | ✓       | ✗         | ✗     | ✗        | ✗               | by string         | ✗          | ✗          | ✗         | ✓                |
| `SimpleNamespace` | ✓       | ✗         | ✗     | ✗        | ✗               | ✓                 | ✗          | ✗          | ✗         | ✓                |
| plain class       | ✓       | manual    | ✓     | ✓        | ✓               | ✓                 | manual     | manual     | ✓         | ✓                |
| `dataclass`       | ✓       | ✓         | ✓     | ✓        | ✓               | ✓                 | ✗          | ✗          | ✓         | ✓                |
| `attrs`           | ✓       | ✓         | ✓     | ✓        | ✓               | ✓                 | ✓          | ✓          | ✓         | ✗                |
| `pydantic`        | ✓       | ✓         | ✓     | ✓        | ✓               | ✓                 | ✓          | ✓          | ✓         | ✗                |

## The options

### `tuple`

Fast and memory-efficient, but fields are reached by position, which is easy to get wrong. A tuple
is immutable and carries no type hints.

```python
x = 42, 4.5, "hello"
y = x[0]  # by index only
```

### `namedtuple`

Adds names, so fields can be read as `x.n` instead of `x[0]`. Still immutable and untyped.

```python
from collections import namedtuple

T = namedtuple("T", ["n", "f", "s"])

x = T(42, f=4.5, s="hello")
y = x[0]  # by index
y = x.n  # or by name
```

### `NamedTuple`

The `typing` version of `namedtuple`, declared as a class with type hints. It is still a tuple, so
a plain untyped tuple can be mixed in where a `T` is expected without an error.

```python
from typing import NamedTuple


class T(NamedTuple):
    n: int
    f: float
    s: str


x = T(42, f=4.5, s="hello")
y = x.n
```

### `dict`

Mutable and built in, but keys are strings, so a typo fails only at runtime, and there is no type
checking.

```python
x = {"n": 42, "f": 4.5, "s": "hello"}

y = x["n"]  # a mistyped key fails at runtime
x["n"] = 0
```

### `SimpleNamespace`

Like a bare `object`, except attributes can be added at runtime. It takes keyword arguments only.

```python
from types import SimpleNamespace

x = SimpleNamespace(n=42, f=4.5, s="hello")
y = x.n
x.n = 0
```

### `dataclass`

Generates `__init__`, `__repr__` and comparisons from type hints. It supports defaults, default
factories for mutable values and frozen instances, but no converters or validators. Since Python
3.10 it can also use slots.

```python
from dataclasses import dataclass


@dataclass(slots=True)
class T:
    n: int
    f: float
    s: str


x = T(42, f=4.5, s="hello")
y = x.n
x.n = 0
```

### `attrs`

Everything `dataclass` does, plus optional converters and validators per field. It is a
third-party package. mCoding reaches for it most in real projects: that extra flexibility can make
a big difference.

```python
import attr


@attr.s
class T:
    n: int = attr.ib(converter=int)
    f: float = attr.ib(validator=attr.validators.instance_of(float))
    s: str = attr.ib(default="")
    l: list = attr.ib(factory=list)


x = T(42, f=4.5, s="hello")
y = x.n
x.n = 0
```

### `pydantic`

Built for one job, parsing: every argument is converted to its declared type and checked at
runtime. That is ideal for untrusted input such as JSON from a web API, and wasted time and memory
for data your own program creates, where a static type checker catches the same mistakes before
the code runs. It is not a general replacement for `dataclass`.

```python
from pydantic import BaseModel


class T(BaseModel):
    n: int
    f: float
    s: str


x = T(n=42, f=4.5, s="hello")  # keyword arguments only; converted and checked
y = x.n
x.n = 0
```

## Speed and memory

- **Creating an instance.** `tuple` and `dict` are fastest, because they are written mostly in C;
  everything else goes through Python.
- **Reading an attribute.** `SimpleNamespace` and `pydantic` are the slowest.
- **Setting an attribute.** All the mutable options take about the same time.
- **Memory.** Tuple-based and slotted instances stay under 200 bytes; anything with an instance
  `__dict__` goes over.

## Which one to use

- **Best overall:** `attrs`.
- **Most convenient:** `dataclass`, since it ships with Python.
- **Record-like data:** `NamedTuple`.
- **Parsing external data:** `pydantic`.

[^mcoding]: [mCoding: Which Python @dataclass is best? Feat. Pydantic & NamedTuple & attrs...](https://youtu.be/vCLetdhswMg)

