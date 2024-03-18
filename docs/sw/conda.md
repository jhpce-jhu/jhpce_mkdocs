## Create an environment
- load conda module
```
module load conda
```
or  
```
module load anaconda
```

- create a new environment
```
conda create -n <evn-name>
```

## list environments
- lista all your conda environment
```
conda info --envs
```

!!! Tip "Tip"
    The active environment is the one with an asterisk (*)

## Install a conda package in the new created environment
```
conda activate env-name
conda install package-name
```

!!! Note "Notes:"  
    To install a specific version:  
    ```
    conda install package-name=2.3.4
    ```  
    To specify only a major version
    ```
    conda install package-name=2
    ```
 
## specifying channels to use
- Channels are locations where packages are stored. By default, conda searchs for packages in its default channels. You can specify a channel when installing the package (e.g. conda-forge channel)
```
conda install conda-forge::numpy
```

