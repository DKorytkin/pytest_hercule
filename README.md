# pytest_hercule
Pytest plugin which help to find coupled tests.

Sometimes we have coupled tests which depend from ordering

For example:
- *PASSES* `tests/exmaple/test_all_read.py tests/exmaple/test_b_modify.py tests/exmaple/test_c_delete.py`
- *FAILED* `tests/exmaple/test_c_delete.py tests/exmaple/test_b_modify.py tests/exmaple/test_all_read.py`

In this case pretty simple to detect coupled tests, but if we have >=1k tests which called before it will hard


## Content:
- [install](#install)
- [how to use](#how-to-use)
- [waiting in the future](#todo)

### Install
```bash
pip install pytest_hercule
```

### how to use:
```bash
pytest tests/exmaple/test_c_delete.py tests/exmaple/test_b_modify.py tests/exmaple/test_all_read.py --flaky-test="test_read_params" -vv -x
```
Plugin didn't run all tests, it try to find some possible guilty test and will run first
```bash
======================================================================================== test session starts ========================================================================================
platform darwin -- Python 2.7.16, pytest-3.5.1, py-1.8.1, pluggy-0.6.0 -- /Users/denis.korytkin/.virtualenvs/pytest_hercule/bin/python
cachedir: .pytest_cache
rootdir: /Users/denis.korytkin/workspace/pytest_hercule, inifile:
collected 3 items                                                                                                                                                                                   
Try to find coupled tests:

tests/exmaple/test_b_modify.py::test_modify_random_param PASSED                                                                                                                               [ 33%]
tests/exmaple/test_all_read.py::test_read_params FAILED                                                                                                                                       [ 66%]
tests/exmaple/test_c_delete.py::test_delete_random_param PASSED                                                                                                                               [100%]

============================================================================================= FAILURES ==============================================================================================
```
Also you can use `pytest -x` or `--exitfirst`
```bash
======================================================================================== test session starts ========================================================================================
platform darwin -- Python 2.7.16, pytest-3.5.1, py-1.8.1, pluggy-0.6.0 -- /Users/denis.korytkin/.virtualenvs/pytest_hercule/bin/python
cachedir: .pytest_cache
rootdir: /Users/denis.korytkin/workspace/pytest_hercule, inifile:
collected 3 items                                                                                                                                                                                   
Try to find coupled tests:

tests/exmaple/test_b_modify.py::test_modify_random_param PASSED                                                                                                                               [ 33%]
tests/exmaple/test_all_read.py::test_read_params FAILED                                                                                                                                       [ 66%]

============================================================================================= FAILURES ==============================================================================================
```

### TODO
I have a couple ideas, how to improve finder coupled tests:
- use AST for detect common peace of code (variables, functions, etc...)
- run not all tests (binary search algorithm)
- Also need to add tests for it =)