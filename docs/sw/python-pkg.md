## Installing Python Packages

- Choose the Python version you want to use
```
[bob@compute-107 /users/bob]$ module load python
[bob@compute-107 /users/bob]$ python3 --version
Python 3.9.14
```

!!! Note "Note"
    You can use `module avail python` to see available python versions on the cluster, and load the one you want to use.
    
- Ensure you can run pip from the command line
```
[bob@compute-107 /users/bob]$ python3 -m pip --version
pip 23.1.2 from /jhpce/shared/jhpce/core/python/3.9.14/lib/python3.9/site-packages/pip (python 3.9)
```

- Installing a package into your own home directory (e.g. the "cowsay" package)
```
[bob@compute-107 /users/bob]$ python3 -m pip install --user cowsay
```

When you install a python package, the program will be installed into youur local Python bin directory, which by defaul is the directory ".local/bin" under your home directory.  In order to easily use the program, you will want to add this directory to your PATH environment variable.  If you don't have the ".local/bin" in your path, you will likely get a warning message when you "pip install" the program, and also likely get a "command not found" error when trying to run the command.
```
[bob@compute-107 /users/bob]$ python3 -m pip install --user cowsay
Collecting cowsay
  Downloading cowsay-6.1-py3-none-any.whl (25 kB)
Installing collected packages: cowsay

. . .

WARNING: The script cowsay is installed in '/users/bob/.local/bin' which is not on PATH.
Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.

. . .

Successfully installed cowsay-6.1
[bob@compute-107 /users/bob]$ cowsay -t "Hello"
-bash: cowsay: command not found
```
In order to easily run the program, you would need to add your local Python bin directory to your PATH environment variable.
```
[bob@compute-107 /users/bob]$ echo $PATH
/jhpce/shared/jhpce/core/JHPCE_tools/3.0/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin
[bob@compute-107 /users/bob]$ export PATH=$PATH:$HOME/.local/bin
[bob@compute-107 /users/bob]$ cowsay -t "Hello"
  _____
| Hello |
  =====
     \
      \
        ^__^
        (oo)\_______
        (__)\       )\/\
            ||----w |
            ||     ||
[bob@compute-107 /users/bob]$ 
```
Setting your PATH this way will only be effective for your current session, so if you log out, you would need to set your PATH again.  To make this change to your PATH permanent you can use a text editor to edit your the .bash_profile file in your home directory and add the "export PATH=$PATH:$HOME/.local/bin" to the end of that file, or use the commands below to make a backup copy of your .bash_profile file, and append the PATH line to the end of the .bash_profile file:
```
[bob@compute-107 /users/bob]$ cp $HOME/.bashrc $HOME/.bashrc-bak
[bob@compute-107 /users/bob]$ echo "export PATH=\$PATH:\$HOME/.local/bin" >> $HOME/.bash_profile
```

!!! Note "Notes:"  
    To install a specific version:  
    ```
    [bob@compute-107 /users/bob]$ python3 -m pip install --user cowsay==5.0
    ```
