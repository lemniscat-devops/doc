## Requirements
The runtime requires Python 3.10 or later and the following packages:
`setup-tools` in version 61.0.0 or later.

### Check the Python version

Check if you have the correct version of Python installed. To do this, open a terminal and run the following command:

on Windows:
```bash
py --version
```

on Linux:
```bash
python3 --version
```
### Check the setup-tools package

Check if you have the `setup-tools` package installed. To do this, open a terminal and run the following command:

on Windows:
```bash
py -m ensurepip --upgrade
pip show setuptools
```

on Linux:
```bash
python3 -m ensurepip --upgrade
pip show setuptools
```

If the package is not installed, you can install it with the following command:

```bash
pip install setuptools
```

If the package is installed, but not up to date, you can update it with the following command:

```bash
pip install --upgrade setuptools
```


## Install the runtime

To install the runtime, you can use the following command:

```bash
pip install lemniscat-runtime
```