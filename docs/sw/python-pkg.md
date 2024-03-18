## Installing Python Packages

- Choose the Python version you want to use
```
[test@compute-107 ~]$ module load python
[test@compute-107 ~]$ python3 --version
Python 3.9.14
```

- Ensure you can run pip from the command line
```
[compute-107 /users/test]$ python3 -m pip --version
pip 23.1.2 from /jhpce/shared/jhpce/core/python/3.9.14/lib/python3.9/site-packages/pip (python 3.9)
```

- Installing a package into your own home directory (e.g. requests package)
```
[test@compute-107 ~]$ python3 -m pip install --user requests
```

!!! Note "Notes:"  
    To install a specific version:  
    ```
    [test@compute-107 ~]$ python3 -m pip install --user requests=2.31.0
    ```
