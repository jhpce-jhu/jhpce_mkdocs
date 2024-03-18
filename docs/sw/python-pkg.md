## Installing Python Packages

- Choose the Python version you want to use
```
[teset@compute-107 ~]$ module load python
[teset@compute-107 ~]$ python3 --version
Python 3.9.14
```

- Ensure you can run pip from the command line
```
[compute-107 /users/test]$ python3 -m pip --version
pip 23.1.2 from /jhpce/shared/jhpce/core/python/3.9.14/lib/python3.9/site-packages/pip (python 3.9)
```

- Installing a package into your own home directory (e.g. requests package)
```
[teset@compute-107 ~]$ python3 -m pip install --user requests
```

!!! Note "Notes:"  
    To install a specific version:  
    ```
    [teset@compute-107 ~]$ python3 -m pip install --user requests=2.31.0
    ```
