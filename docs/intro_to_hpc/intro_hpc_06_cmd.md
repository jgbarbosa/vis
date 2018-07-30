### Using command line to launch Remote Desktop Access

#### Start a Remote Desktop

TACC has provided a VNC job script (/share/doc/slurm/job.vnc) that requests one node in the vis queue for four hours, creating a VNC session.
```
$ sbatch /share/doc/slurm/job.vnc
```
You may modify or overwrite script defaults with sbatch command-line options:

"-t hours:minutes:seconds" modifies the job runtime
"-A projectnumber" specifies the project to be charged
"-N nodes" sets the number of nodes needed
"-p partition" to specify an alternate partition (queue).
See more sbatch options in Stampede User Guide Table 7.3

All arguments after the job script name are sent to the vncserver command. For example, to set the desktop resolution to 1440x900, use:
login1$ sbatch /share/doc/slurm/job.vnc -geometry 1440x900
The vnc.job script starts a vncserver process and writes to the output file, vncserver.out in the job submission directory, with the connect port for the vncviewer. Watch for the "To connect via VNC client" message at the end of the output file, or watch the output stream in a separate window with the commands:
```
$ touch vncserver.out ; tail -f vncserver.out
```
The lightweight window manager, xfce, is the default VNC desktop and is recommended for remote performance. Gnome is available; to use gnome, open the "~/.vnc/xstartup" file (created after your first VNC session) and replace "startxfce4" with "gnome-session". Note that gnome may lag over slow internet connections.

#### Create an SSH Tunnel to stampede2

TACC requires users to create an SSH tunnel from the local system to the stampede2 login node (stampede2.tacc.utexas.edu) to assure that the connection is secure. On a Unix or Linux system, execute the following command once the port has been opened on the stampede2 login node:
```
$ ssh -f -N -L xxxx:stampede2.tacc.utexas.edu:yyyy username@stampede2.tacc.utexas.edu
```
where

|    |     |
|----|-----|
|"yyyy" | is the port number given by the vncserver batch job |
|"xxxx" | is a port on the remote system. Generally, the port number specified on the stampede2 login node, yyyy, is a good choice to use on your local system as well |
|"-f" | instructs SSH to only forward ports, not to execute a remote command |
|"-N" | puts the ssh command into the background after connecting |
|"-L" | forwards the port |

On Windows systems find the menu in the Windows SSH client where tunnels can be specified, and enter the local and remote ports as required, then ssh to stampede2.

#### Connecting vncviewer

Once the SSH tunnel has been established, use a VNC client to connect to the local port you created, which will then be tunneled to your VNC server on stampede2. Connect to localhost:xxxx, where xxxx is the local port you used for your tunnel. In the examples above, we would connect the VNC client to localhost::xxxx. (Some VNC clients accept localhost:xxxx).

We recommend the [TigerVNC VNC Client](http://tigervnc.org/), a platform independent client/server application.

Once the desktop has been established, two initial xterm windows are presented (which may be overlapping). One, which is white-on-black, manages the lifetime of the VNC server process. Killing this window (typically by typing "exit" or "ctrl-D" at the prompt) will cause the vncserver to terminate and the original batch job to end. Because of this, we recommend that this window not be used for other purposes; it is just too easy to accidentally kill it and terminate the session.

The other xterm window is black-on-white, and can be used to start both serial programs running on the node hosting the vncserver process, or parallel jobs running across the set of cores associated with the original batch job. Additional xterm windows can be created using the window-manager left-button menu.

Previous: [Visualization on stampede2](intro_to_hpc_06.md) | Top: [Course Overview](../../index.md)
