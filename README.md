# Troubleshooting for the IRIS Demo Applications

This document aggregates troubleshooting guidance for all the IRIS based demo applications.

## Docker related problems

### Problem 1 - No space left on device

If when trying to run a demo you see a message like this:

```
Stopping and deleting previous containers...
No stopped containers
Creating network "irisdemo-demo-fraudprevention_default" with the default driver
Pulling bankingtrnsrv (intersystemsdc/irisdemo-demo-fraudprevention:bankingtrnsrv-version-1.7.0)...
bankingtrnsrv-version-1.7.0: Pulling from intersystemsdc/irisdemo-demo-fraudprevention
898c46f3b1a1: Already exists
63366dfa0a50: Already exists
041d4cd74a92: Already exists
6e1bee0f8701: Already exists
973e47831f38: Already exists
65fde5c4b1e1: Already exists
a1d6b75ab23f: Already exists
c57eccc19d7f: Already exists
e42087247279: Already exists
f2615d4cbea0: Pull complete
ab62cb6b47e2: Pull complete
2c525bbf4e0e: Pull complete
f9f962189312: Pull complete
5d89a7358b04: Pull complete
f8d8b27e662b: Pull complete
c48e017b94fb: Pull complete
38aedf41f471: Extracting [==================================================>]  170.1MB/170.1MB
0230d1324dc3: Download complete
bd6ccf98d45b: Download complete
8c85e277e9c4: Download complete
264301b7eb8c: Download complete
1081256a451c: Download complete
fe994e56c5bc: Download complete
b01dae550871: Download complete
ERROR: failed to register layer: Error processing tar file(exit status 1): write /usr/irissys/mgr/irislib/IRIS.DAT: no space left on device
```

Your Docker Machine is running out of disk space. If you are using:
* **Docker on Windows or Mac** - There is a Linux Virtual Machine (VM) that docker uses to run the containers. This VM may be running out of space.
* **Linux** - There is no VM. Docker runs natively on your system. Your Linux system is running out of space. 

You can see how much space you are giving to your Docker VM on Windows or Mac by asking to see Docker's **Settings** and then clicking on **Advanced**. Look for something like *"disk image max size"*. 

You can then either increas the size of your Docker's VM or try to kill some unused images and/or containers using the **docker** command line. A simple way of eliminating many unused images and containers is with the command **docker system prune** command.

After increasing the size of your docker's VM and/or eliminating unused images/containers, you can try to start the application again.

### Problem 2 - Cannot create container for service X

If you see an error like this on your Windows machine:

```
ERROR: for irisdemo-demo-readmission_risksrv_1  Cannot create container for service risksrv: status code not OK but 500: {"Message":"Unhandled exception: Drive has not been shared"}
ERROR: for risksrv  Cannot create container for service risksrv: status code not OK but 500: {"Message":"Unhandled exception: Drive has not been shared"}
ERROR: Encountered errors while bringing up the project.
```

That is probably because you have not given Docker permission to access your windows folder. Normally, when you start a docker container or a composition that is using a shared volume, Docker will trigger a Windows prompt for giving Docker permissions to access these folders. If you have not given Docker permission to see your local folders on this opportunity, you can open Docker Settings and find the option to add C:\ (or the root folder where your code is) so that Windows will allow access to it.

### Problem 3 - IRIS Startup Aborted

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

Apply solution for **Problem 1** above.
