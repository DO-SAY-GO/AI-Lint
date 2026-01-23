# The Problem: Syntax vs. Semantics

Correct code is not the same as idiomatic code.

LLMs reliably hit:
* Syntax ✅
* Functionality ✅

They frequently miss:
* Idioms ❌
* Language "feel" ❌
* Architectural intent ❌

## The Uncanny Valley of Code

Consider this Python loop:

```python
for i in range(len(arr)):
    something(arr[i])
```

It runs. It passes tests. But an experienced Python developer would write:

```python
for item in arr:
    something(item)
```

The first example fights the language. The second works with it. This instinct—knowing what *rejected* options look like—is what AI lacks.

## The Missing Context

LLMs have training data, but they lack an authoritative map of what experienced practitioners reject. This knowledge lives in:
* Senior engineers' heads
* Code reviews
* Implicit team culture

AI Lint makes this implicit knowledge explicit. It provides a map of "technically correct but wrong" patterns to help AI choose the best expression for a given language.
