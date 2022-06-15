# Using `direnv`: Specifications for per-directory environments

```{article-info}
:avatar: https://secure.gravatar.com/avatar/709ea66dc102e6bc4547032f85ff6c95 
:avatar-link: mailto:paul.gierz@awi.de 
:avatar-outline: muted
:author: Paul Gierz 
:date: June 2022
:read-time: "15 min read and hands-on"
:class-container: sd-p-2 sd-outline-muted sd-rounded-1
```

[`direnv`](https://www.direnv.net) allows you to specify particular actions your shell
will perform when entering or exiting a particular directory.

This tutorial will give you an overview of how to set up `direnv` on the
various supercomputers that `AWI-ESM` currently is running on. As always, a
video guide as well as text documentation is provided.

## Setting up `direnv`

```{important}
The example files for `.bash_profile` and `.bashrc` are considerably reduced in
these examples. You may have much more customization in your own files.
```

```````{tab-set}
``````{tab-item} DKRZ Levante
`````{note}
These instructions are specific to DKRZ's Levante System!
`````
`````{eval-rst}
..  youtube::  L52Qn9sU3sQ
`````
Enable additional module files stored under ``/work/ab0246/AWImodules`` by adding the following to your ``.bashrc`` or ``.bash_profile``:

````{card}
{fas}`fa-file-code` File: `${HOME}/.bashrc` or `${HOME}/.bash_profile`
^^^
```diff
#!/bin/bash

+ module use /work/ab0246/AWImodules 
```
```{warning}
This is a noteable difference to the setup procedure on Ollie, where this step
is **not needed**.
```
````
Load the module for ``direnv``, this can be done by default by adding the following to ``.bashrc`` or ``.bash_profile``:
````{card}
`${HOME}/.bashrc` or `${HOME}/.bash_profile`
^^^
```diff
#!/bin/bash

module use /work/ab0246/AWImodules 
+ module load utils/direnv
```
````
Enable the integration with your shell
````{card}
`${HOME}/.bashrc` or `${HOME}/.bash_profile`
^^^
```diff
#!/bin/bash

module use /work/ab0246/AWImodules 
module load utils/direnv
+ eval "$(direnv hook bash)"
```
````
```{hint}
This is not limited to ``bash``, but can also be done for [other shells](https://direnv.net/docs/hook.html).
```
Finally, you will need to either log out and in again, or source your configuration file:
````{card}
Terminal
^^^
```
$ source ${HOME}/.bash_profile
$ source ${HOME}/.bashrc
```
````
``````










``````{tab-item} AWI Ollie
```{note}
These instructions are specific to AWI's Ollie System!
```
```{eval-rst}
..  youtube:: VCbBMcDQBSI
```
Load the module for ``direnv``, this can be done by default by adding the following to ``.bashrc`` or ``.bash_profile``:

````{card}
`${HOME}/.bashrc` or `${HOME}/.bash_profile`
^^^
```diff
#!/bin/bash

+ module load direnv
```

```{warning}
This is different from Levante! You **do not** need to include a `module use` statement on Ollie.
```


````

Enable the integration with your shell
````{card}
`${HOME}/.bashrc` or `${HOME}/.bash_profile`
^^^
```diff
#!/bin/bash

module load utils/direnv
+ eval "$(direnv hook bash)"
```
````
```{hint}
This is not limited to ``bash``, but can also be done for [other shells](https://direnv.net/docs/hook.html).
```

Finally, you will need to either log out and in again, or source your configuration file:

````{card}
Terminal
^^^
```
$ source ${HOME}/.bash_profile
$ source ${HOME}/.bashrc
```
````
`````{warning}
You might get an error as follows:
````{card}
Terminal
^^^
```
ERROR: unable to locate a modulefile for gcc
```
This can be safely ignored.
````
``````
```````

## Basic Usage of `direnv`
```{eval-rst}
..  youtube:: OCBBqLwLeUc
```

We will start with a basic demonstration of the `direnv` program. 

```{important}
This is the same regardless of the machine you are on.
```

To begin, make a small demo directory where you can play around in and enter it:
```
$ mkdir ${HOME}/direnv_demos
$ cd ${HOME}/direnv_demos
```

Next, create a file `.envrc` that will run as a *new subshell* based upon the
*hook* command you provided above. This will run the commands in your file
whenever you enter this folder:
```
$ direnv edit .
```
Above, note that we had to specify the current working directory to tell
`direnv` which folder should run special commands when entering or exiting it.
```{important}
You need to have the `$EDITOR` variable set in your shell for `direnv edit .`
to work correctly!
```

You can also create this file manually:
```
$ vim .envrc
```
You can use whatever editor you want. 

In this file, we are going to do a few basic examples. First, let's just get a greeting:
````{card}
The `.envrc` File
^^^
```diff
+ echo "Hello from a special file executed for you with direnv!"
```
````
Save and close the file. 
````{hint}
If you manually created this with `vim` or something similar, you will likely get a warning:
```
...
```
`direnv` keeps track of which "special" locations it knows about, and you need
to enable the per-folder functionality. This can be done with:
```
$ direnv allow
```
````
Notice that once you have closed (and, if needed, white-listed) the `.envrc`
file, the commands specified in the `.envrc` file are run. Let's try going
somewhere else and coming back:
````{card}
Terminal
^^^
```
$ cd ${HOME}
direnv unloading
$ cd ${HOME}/direnv_demos
Hello from a special file executed for you with direnv!
$ 
```
````
Let's get a little more complicated. Since we can run basic commands, we can
also load or unload environment modules. Since we already have a `.envrc` file,
we can just use `direnv edit` (without specifying a directory location) to open
up the currently known file.
````{card}
Terminal
^^^
```
$ direnv edit
```
````
Let's add a few things:
````{card}
The `.envrc` file
^^^
```diff
echo "Hello from a special file executed for you with direnv"
+ echo "The following modules are currently loaded:"
+ module list
+ echo "Removing all modules..."
+ module purge
+ echo "Loading git module..."
+ module load git
+ echo "Loaded modules after direnv finishes:"
+ module list
+ echo "The direnv script has finished!"
```
````
If you now save and exit the file, the following should happen in your terminal:
````{card}
Terminal
^^^
```
Hello from a special file executed for you with direnv
The following modules are currently loaded:
No Modulefiles Currently Loaded.
Removing all modules...
Loading git module...
Module for git, git version 2.13.1 loaded.
Loaded modules after direnv finishes:
Currently Loaded Modulefiles:
 1) git/2.13.1
The direnv script has finished!
direnv: export +LD_LIBRARY_PATH +LD_LIBRARY_PATH_modshare +LIBRARY_PATH +LIBRARY_PATH_modshare +LOADEDMODULES_modshare +MANPATH_modshare +PATH_modshare +_LMFILES_ +_LMFILES__modshare ~LOADEDMODULES ~MANPATH ~PATH
```
+++
```{note}
You may of course have a different response instead of `No Modulefiles
Currently Loaded.` depending on your configuration!
```
````

One final element that will be shown in this tutorial is how you can use
`direnv` to automatically generate and activate [virtual `Python` environments](https://realpython.com/python-virtual-environments-a-primer/) for
you. This allows you to have a segregated and distinct set of packages for any
project that is using `direnv`. We are going to replace a bit of the `.envrc` file:
````{card}
Terminal
^^^
```
$ direnv edit
```
````
We will change to the following:
````{card}
The `.envrc` file
^^^
```diff
- echo "Hello from a special file executed for you with direnv"
- echo "The following modules are currently loaded:"
- module list
- echo "Removing all modules..."
- module purge
- echo "Loading git module..."
- module load git
- echo "Loaded modules after direnv finishes:"
- module list
- echo "The direnv script has finished!"
+ module load python3
+ layout python
```
````
The following output should be shown:
````{card}
Terminal
^^^
```
direnv: loading ~/direnv_demos/.envrc
Module for Python 3.7 from Intel Parallel Studio 2020.2.902 loaded
direnv: export +LOADEDMODULES_modshare +MANPATH_modshare +PATH_modshare +POJ_LIB +USE_DAAL4PY_SKLEARN +VIRTUAL_ENV +_LMFILES_ +_LMFILES__modshare ~LOADEDMODULES ~MANPATH ~PATH
```
````
If we inspect the location of `Python`, we now see that we have a unique binary tied to this folder:
````{card}
Terminal
^^^
```
$ which python
~/direnv_demos/.direnv/python-3.7.7/bin/python
```
````
If we exit this folder, we get the default `Python` again:
````{card}
Terminal
^^^
```
$ cd ${HOME}
direnv: unloading
$ which python
/global/AWIsoft/miniconda/4.10.3/bin/python
```
````
## Summary
You should now have an overview of the `direnv` program and how to use it to
create isolated folders. On this page, you learned the following:

* Setting up `direnv` on AWI's Ollie and DKRZ's Levante supercomputers.
* Enabling `direnv` integration to your shell
* Running specific commands when entering a folder
* Setting up shell variables that are only active in a specific folder
* Using the `layout` command provided by the `direnv` standard library to
  enable per-directory Python environments


```{raw} html
<script
   type="text/javascript"
   src="https://utteranc.es/client.js"
   async="async"
   repo="awi-esm/docs"
   issue-term="pathname"
   label="💬 comment"
   crossorigin="anonymous"
/>
```

