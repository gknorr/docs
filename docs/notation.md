# Notation and Formatting
Here we provide a summary of the notation which might be used in the
documentation overall. 

Some of the commands here will be "one-time-only". They will be
marked as follows:
````{admonition} One-Time Command
The following command or edit to a particular configuration file will only need to be applied once! Code will be like this:
```shell
$ cat README.md
These are the contents of a file.
$ for fname in ${list_of_files}; do
    echo "The file ${fname} is currently in the loop body"
  done

```
````
Or, you might need to edit a particular file once only:
````{admonition} One-Time Edit
```diff
#!/bin/bash
#
# This is a `${HOME}/.bashrc` file. You might need to add something to it: 

+ module load ncview 
echo "No one knows why ncview is not a default"
# Or you might need to remove something as well:
- module purge gcc
echo "You should probably have some sort of compiler loaded"
```

````
You will have the situation where a particular configuration file might need to
be changed. Changes to a set of code might be marked like this:
````{admonition} Code Changes
```diff
#!/bin/bash
#
# This is an example bash script file. It doesn't do very much but is an
# demo of how documentation will appear on the website.
#
# Paul Gierz, 2022
+ echo "Add this line"
echo "Here is a context line with no specific change"
- echo "Remove this line"
echo "Some context again"
echo "Even more context lines"
```
````
Commands entered in a specific programming language might have syntax highlighting:
```python
>>> a = 3
>>> b = 5
>>> print(a+b)
8
```
