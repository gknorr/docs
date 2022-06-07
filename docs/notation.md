# Notation and Formatting
Here we provide a summary of the notation which might be used in the documentation overall. There are some of the commands here will be "one-time-only". They will be marked as follows:
````{admonition} One-Time Command
:class: tip
The following command or edit to a particular configuration file will only need to be applied once! Code will be like this:
```shell
$ cat README.md
These are the contents of a file.
$ for fname in ${list_of_files}; do
    echo "The file ${fname} is currently in the loop body"
  done

```
````
Changes to a set of code might be marked like this:
````{admonition} Code Changes
```diff
+ Add this line
Here is a context line with no specific change
- Remove this line
```
````
Commands entered in a specific programming language might have syntax highlighting:
```python
>>> a = 3
>>> b = 5
>>> print(a+b)
8
```
