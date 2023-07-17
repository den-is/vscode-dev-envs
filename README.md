## Building container image
```sh
cd python
docker build --progress=plain -t pydev .
```

## Configuring linters, formaters, etc.
Tools can be configured by:
- **Recommended:** Usually each tool supports per project configuration settings file. Some of the tools comply to [PEP518](https://www.python.org/dev/peps/pep-0518/) and use `pyproject.toml` to store their configuration values. Other tools use their own configuration files that can be placed in projects root, users home, or might have global system configuration. Read tools documentation.
- You can pass VScode configuration settings devcontainer.json. Usually these are command line arguments for tools that VScode will send to a tool on invocation. Example:
  ```ts
    "settings": {
      "python.defaultInterpreterPath": "/usr/local/bin/python",
      "python.linting.enabled": true,
      "python.linting.pylintEnabled": false,
      "python.linting.flake8Enabled": true,
      "python.linting.pylintArgs": [
        // "--disable=missing-docstring,unused-import",
        "--max-line-length=120",
        "--disable=C0111,W0611"
      ],
      "python.linting.flake8Args": [
        "--max-line-length=120",
        "--ignore=E402,F841,F401,W503"
      ],
    }
  ```

## References
- [Visual Studio Code: Developing inside a Container](https://code.visualstudio.com/docs/remote/containers)
- [VSCode Remote Container Definitions](https://github.com/microsoft/vscode-dev-containers)
- [VSCode Remote host / container / WSL Linux prerequisites](https://code.visualstudio.com/docs/remote/linux#_remote-host-container-wsl-linux-prerequisites)
- [Development Containers specification](https://containers.dev/)
- [Development Containers official repo](https://github.com/devcontainers)
