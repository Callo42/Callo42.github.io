---
layout: post
title: Using FLAGS in absl python test case
date: 2023-10-28 09:41 +0800
categories: [Gleanings, Technical Supports, Python]
tags: [absl, python]
math: False
---

## Using FLAGS in a single test case while using absl python

While using `absl python` and making tests, one may encounter
the situation to access `FLAGS` while a test case is invoked.

This could typically be done with

```python
from absl.testing import absltest
from absl.testing import flagsaver

class TestFoo(absltest.TestCase)

    @flagsaver.as_parsed(
        some_flag='False',
        another_flag='5.0'
        )
    def test_some_func(self):
        do_stuff()
```

This could to some extend solve the error such as 
`flags unparsed error`, etc.

For more information, see the github source code 
[here](https://github.com/abseil/abseil-py/blob/main/absl/testing/flagsaver.py)
