# Python Development Environment

## Preinstalled python dev tools
- black
- isort
- flake8
  - flake8-bugbear
  - flake8-flask
  - flake8-isort
  - flake8-comprehensions
  - pep8-naming
- pylint
- yapf
- autopep8
- mypy
- pydocstyle
- bandit
- semgrep
- pipenv
- virtualenv

## isort vscode configuration issues
As by this github [issue](https://github.com/microsoft/vscode-python/issues/7042) VScode is not able to properly configure isort using vscode settings. Because vscode has old outdated isort dependency. For example this is not working `"python.sortImports.args": ["--profile=black"]`.

Solution is to include `.isort.cfg` in your project's root. That Way isort will be able to fetch proper configuration settings.
Example isort config file contents:
```ini
[settings]
profile=black
skip_gitignore=true
```
