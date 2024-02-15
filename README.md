# Guide to install Python Libraries Offline

This is a remake of a guide I've made long time ago. This is particularly useful when you have a server on an intranet behind a firewall who denies access to PyPI.

Also, using the similar principles, you can build your application and dependencies for a serverless deployment on AWS Lambda and Azure Functions. I'll explain further later on.

## First Step

Create a folder (`venv_libs` here) and inside of it run:

```shell 
python -m pip download \
    --platform manylinux1_x86_64 \
    --python-version 3.6.8 \
    --only-binary=:all: \
    --no-binary=:none: pandas
```

### Platform

The `--platform` depends on the OS of your offline machine, we used `manylinux1_x86_64` on a server with RedHat. For more information about which parameter you should use, take a look at the [manylinux repository](https://github.com/pypa/manylinux)

## Move folder 

Move folder to the offline machine

## Create y activate virtual environment

```shell
python3 -m venv venv
source venv/bin/activate
```


## Install Libraries

```shell
python -m pip install \
    --no-index \
    --find-links=venv_libs/ pandas
```

## References
- [pip documentation](https://pip.pypa.io/en/stable/cli/pip_download/)
- [AWS Docs: Working with .zip file archives for Python Lambda functions](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html)
- [manylinux](https://github.com/pypa/manylinux)

- [StackOverflow: How to install packages offline?](https://stackoverflow.com/questions/11091623/how-to-install-packages-offline)

- [Azure Docs: Deployment Azure Functions](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-python?tabs=asgi%2Capplication-level&pivots=python-mode-decorators#install-local-packages)