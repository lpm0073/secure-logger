# Secure Logger

[![Source code](https://img.shields.io/static/v1?logo=github&label=Git&style=flat-square&color=brightgreen&message=Source%20code)](https://github.com/lpm0073/secure-logger)
[![Forums](https://img.shields.io/static/v1?logo=discourse&label=Forums&style=flat-square&color=000000&message=discuss.openedx.org)](https://discuss.openedx.org/tag/cookiecutter)
[![Documentation](https://img.shields.io/static/v1?&label=Documentation&style=flat-square&color=000000&message=Documentation)](https://github.com/lpm0073/secure-logger)
[![PyPI releases](https://img.shields.io/pypi/v/secure-logger?logo=python&logoColor=white)](https://pypi.org/project/secure-logger)
[![AGPL License](https://img.shields.io/github/license/overhangio/tutor.svg?style=flat-square)](https://www.gnu.org/licenses/agpl-3.0.en.html)
[![hack.d Lawrence McDaniel](https://img.shields.io/badge/hack.d-Lawrence%20McDaniel-orange.svg)](https://lawrencemcdaniel.com)

A Python decorator to generate redacted and nicely formatted log entries of class instantiations, class method calls and standard Python function calls. Redacts function parameter values and dict values based on a customizable list.

Redacts the following values by default:

```python
DEFAULT_SENSITIVE_KEYS = [
    "password",
    "token",
    "client_id",
    "client_secret",
    "Authorization",
    "secret",
    "aws_access_key_id",
    "aws_secret_access_key",
]
```

## Usage

```bash
pip install secure-logger
```

```python
from secure_logger.decorators import app_logger

MY_SENSITIVE_KEYS = ["top-secret-password", "equally-secret-value",]

class TestClass(object):

    @app_logger(sensitive_keys=MY_SENSITIVE_KEYS, indent=4)
    def test_2(self, test_dict, test_list):
        pass

test_dict = {
    'insensitive_key': 'you-can-see-me',
    'top-secret-password': 'i-am-hidden',
    'equally-secret-value': 'so-am-i'
}
test_list = ['foo', 'bar']
o = TestClass()
o.test_2(test_dict=test_dict, test_list=test_list)
```

LOG OUTPUT
INFO:app_logger: __main__.TestClass().test_2()  keyword args: {
    "test_dict": {
        "insensitive_key": "you-can-see-me",
        "top-secret-password": "*** -- REDACTED -- ***",
        "equally-secret-value": "*** -- REDACTED -- ***"
    },
    "test_list": [
        "foo",
        "bar"
    ]
}

### Documentation

Documentation is available here: [Documentation](https://github.com/lpm0073/secure-logger)

We welcome contributions! secure-logger is part of the [lpm0073](https://github.com/lpm0073) project. Pull requests are welcome in all repos belonging to this organization. You can also contact [Lawrence McDaniel](https://lawrencemcdaniel.com/contact) directly.

### Getting Started With Local development

- Use the same virtual environment that you use for edx-platform
- Ensure that your Python interpreter to 3.8x
- install black: <https://pypi.org/project/black/>
- install flake8: <https://flake8.pycqa.org/en/latest/>
- install flake8-coding: <https://pypi.org/project/flake8-coding/>

```bash
# Run these from within your edx-platform virtual environment
python3 -m venv venv
source venv/bin/activate

pip install -r requirements/local.txt
pip install pre-commit black flake8
pre-commit install
```

#### Local development good practices

- run `black` on modified code before committing.
- run `flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics`
- run `flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics`
- run `pre-commit run --all-files` before pushing. see: <https://pre-commit.com/>

#### edx-platform dependencies

To avoid freaky version conflicts in prod it's a good idea to install all of the edx-platform requirements to your local dev virtual environment.

- requirements/edx/base.txt
- requirements/edx/develop.txt,
- requirements/edx/testing.txt

At a minimum this will give you the full benefit of your IDE's linter.
