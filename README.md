# Troubleshooting for the IRIS Demo Applications

This document aggregates troubleshooting guidance for all the IRIS based demo applications.

## Docker related problems

### IRIS Startup Aborted

If you see the following when trying to start the application:

```
Startup aborted.
  | Startup error. See messages.log for more information.
  | Call InterSystems Technical Support if you need assistance.
  |
  | [ERROR] sh: echo: I/O error
  | sh: echo: I/O error
  |
  | [ERROR] Command "iris start IRIS quietly" exited with status 256
  | [ERROR] Possible causes:
  | [ERROR]  - InterSystems IRIS was not installed successfully
  | [ERROR]  - Invalid InterSystems IRIS instance name
  | [ERROR]  - Insufficient privilege to start InterSystems IRIS (proc not in InterSystems IRIS group?)
  | [FATAL] Error starting InterSystems IRIS
```

It is probably because your Docker Machine is running out of disk space. If you are using:
* Docker on Windows or Mac - There is a Linux Virtual Machine (VM) that docker uses to run the containers. This VM may be running out of space.
* Linux - There is no VM. Docker runs natively on your system. Maybe you are using Linux inside a VM on your Windows/Mac system. The point is that yout Linux system may be running out of space.

To verify if that is really the problem, you may run **docker system df** command to see how much space your images and containers are using. Contrast that with the size of your VM's disk. You can see how much space you are giving to your Docker VM on Windows or Mac by asking to see Docker's **Settings** and then clicking on **Advanced**. Look for something like *"disk image max size"*. 

You can then either increas the size of your Docker's VM or try to kill some unused images and/or containers using the **docker** command line. A simple way of eliminating many unused images and containers is with the command **docker system prune**.

After increasing the size of your docker's VM and/or eliminating unused images/containers, you can try to start the application again.