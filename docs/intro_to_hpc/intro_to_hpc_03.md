## Modules

Environment variables are finicky, prone to typos, and a lot of work to edit manually. *Modules* make dynamically changing environment variables a lot easier and safer. Modules contain all the necessary environment variables for running a particular application or providing access to a particular library. In addition:

* They are a convenient way to dynamically change the user’s environment
* They help avoid conflicts between program versions, compilers, libraries, etc.

Different clusters have different implementations of modules. More info on the TACC implementation of modules can be found [here](https://www.tacc.utexas.edu/research-development/tacc-projects/lmod)


To list all of your currently loaded modules:
```
$ module list
```

To list modules that are available for you to load, do:
```
$ module avail
```

There are quite a few! Modules are organized both by version, and by compiler dependencies (more on this later). To filter, search for modules either by the name of the application, or a keyword. For example:
```
$ module avail paraview/5.2.0
$ module spider paraview/5.2.0
```

Each module, when loaded, will modify your environment variables so that you can run the desired application. To see what the effect of loading a module is, do:
```
$ module show paraview/5.2.0
```

Once you have identified the module you are interested in, you can `load` it to put it in your `PATH`, and `unload` it to remove it from your `PATH`:
```
$ module list
$ module load qt/4.8.4 paraview/5.2.0
$ module list
$ module unload paraview/5.2.0 qt/4.8.4
$ module list
$ ml paraview/5.2.0              # ml is a shortcut for module load
```
Paraview is a graphic visualization tool that runs in X11, we will see later how to launch a VNC session. However Paraview requires a Graphical User Interface (GUI) framework called QT. If you try just "module load paraview" this message will appear:
```
Lmod has detected the following error:  Cannot load module "paraview/5.2.0". At least
one of these module(s) must be loaded:
   qt/4.8.4

While processing the following module(s):
    Module fullname  Module Filename
    ---------------  ---------------
    paraview/5.2.0   /opt/apps/intel15/mvapich2_2_1/modulefiles/paraview/5.2.0
```


Finally, find more help on using module commands by doing:
```
$ module help
```


### Exercise

1. Search the module system for any application(s) that you need for your research.
2. Figure out if any dependencies are required to load the module.
3. Load the module and determine what effect it has on your environment.
4. Make sure the desired executables (and correct version) are in your PATH.

[Click here for solution](intro_to_hpc_03_solution.md)

### Review of Topics Covered

| Command               | Effect     |
|-----------------------|------------|
| `module list`         | list currently loaded modules |
| `module avail`        | see what modules are available |
| `module avail name`   | search for module "name" |
| `module spider name`  | search for module "name" |
| `module key keyword`  | search for modules with a keyword |
| `module show name`    | show the contents of module "name" |
| `module load name`    | load module "name" |
| `ml name`             | ml is a shortcut for module load |
| `module unload name`  | unload module "name" |
| `module help`         | show module command help |


Previous: [Environment Variables](intro_to_hpc_02.md) | Next: [The .bashrc](intro_to_hpc_04.md) | Top: [Course Overview](../../index.md)
