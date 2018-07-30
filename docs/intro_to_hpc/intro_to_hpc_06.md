## Visualization on stampede2

While batch visualization can be performed on any stampede2 node, a set of nodes have been configured for hardware-accelerated rendering. The vis queue contains a subset of 132 compute nodes configured with one NVIDIA K40 GPU each.

### Launch Remote Desktop

#### [From the viz portal](intro_hpc_06_vp.md)

#### [Using command line](intro_hpc_06_cmd.md)

### Using OpenGL application

Running OpenGL/X applications on stampede2 visualization nodes requires that the native X server be running on each participating visualization node. Like other TACC visualization servers, on stampede2 the X servers are started automatically on each node.
Once native X servers are running, several scripts are provided to enable rendering in different scenarios.

* __swr__: Because VNC does not support OpenGL applications, OpenSWR is used to intercept OpenGL/X and use a Intel optimized software rasterizer to render the image; rendered results are then automatically read back and sent to VNC as pixel buffers. To run an OpenGL/X application from a VNC desktop command prompt:
```
$ swr glxgears
```

An example with paraview and paraview servers (a more concrete example will be given later):

First let us load all the modules required to run paraview:
```
$ module load swr python qt paraview
```
While the vglrun is already available by default on the systems, OpenSWR must be explicitly loaded. As said before paraview depends on the QT module and we are loading python to enable scripts inside paraview.

To launch the Paraview frontend, open a two new terminal in the X environment by typing the following command:
```
$ xterm & xterm &
```
Two new windows will pop. In one of the new terminals type:
```
$ swr paraview
```
which will star the paraview from end.

On the other terminal we are going to launch paraview processing and rendering engines by typing:
```
$ ibrun swr pvserver
```
The details of how to use paraview and how to connect to the paraview servers can be found [here]()

### Other systems

* __vglrun__: Because VNC does not support OpenGL applications, VirtualGL is used to intercept OpenGL/X commands issued by application code and re-direct it to a local native X display for rendering; rendered results are then automatically read back and sent to VNC as pixel buffers. To run an OpenGL/X application from a VNC desktop command prompt:
```
$ vglrun glxgears
```

* __tacc_xrun__: Some visualization applications present a client/server architecture, in which every process of a parallel server renders to local graphics resources, then returns rendered pixels to a separate, possibly remote client process for display. By wrapping server processes in the tacc_xrun wrapper, the $DISPLAY environment variable is manipulated to share the rendering load across the two GPUs available on each node. For example,
```
ibrun tacc_xrun application application-args
```
will cause the tasks to utilize each node, but will not render to any VNC desktop windows.

* __tacc_vglrun__: Other visualization applications incorporate the final display function in the root process of the parallel application. This case is much like the one described above except for the root node, which must use vglrun to return rendered pixels to the VNC desktop. For example,
```
$ ibrun tacc_vglrun application application-args
```
will cause the tasks to utilize the GPU for rendering, but will transfer the root process' graphics results to the VNC desktop.

Previous: [Batch Job Submission](intro_to_hpc_05.md) | Top: [Course Overview](../../index.md)
