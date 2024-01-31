# cgroups & Namespaces;

<img alt="meme.png" src="assets/meme.png" />

>[!NOTE]
> Focused more on Docker here [DockerInDeep](https://github.com/Mohamed-abdalazez/DockerInDeep); take a look there as well.

### Table of content

1. [What Are Namespaces? ```overview```.](#desc0)
2. [Proof that containers are just processes.](#desc1)
4. [Namespaces in the Linux Kernel.](#desc2)
5. [A Practical Look at How Containerization Tools Utilize the Namespace Feature in the Isolation Process.](#desc3)
6. [A closer look at some of the types of namespaces.](#desc4)
<a name="desc0"></a>
### What Are Namespaces? ```overview```.

<img alt="Why.png" src="assets/Why.png" />

##### So, let's explore the technical definition for namespaces:- 

- **Namespaces** are a feature of the Linux kernel that partitions kernel resources. This allows one set of processes to see a distinct set of resources, while another set of processes see a different set of resources.

> [!TIP]
> So, since the key feature of namespaces is to isolate processes from each other, this takes us to the container term.<br>
> Using containers in development gives us an isolated environment  that acts like a virtual machine. However, it's not a virtual machine; it's just a process running on a server. <br>
> Take a look [here](https://github.com/Mohamed-abdalazez/DockerInDeep#crucial-concept-between-a-virtual-machine-and-a-container) for a crucial concept comparing a virtual machine and a container.


<a name="desc1"></a>
### Proof that containers are just processes.

- When you start a Docker container, you're actually starting a Linux process. The container runtime then use the already existing Linux features to provide isolation.***More details in the next sections :D***

- So let's discover this real scenario:
  
<img src="assets/Containers_Are_Just_Processes.png"><br>

##### Initial Check.
   - Ran ```ps``` command to check processes related to nginx.**_nothing._**
##### Docker Container Creation.
   - Executed ```docker run``` command to create a Docker container named safrotWebServer running the nginx image in detached mode ```-d```.
##### Check Nginx processes again and get the process ID for the Nginx master process.
##### Access **safrotWebServer** Container.
   - Used ```docker exec -it``` to access the bash shell inside the **safrotWebServer** Container.
##### Created a new file named safrot_new_file inside the container.
##### 6. Exit the Docker container, navigate to the Nginx process directory, which we just obtained in step 3.
##### List Contents of Nginx Process Root Directory
   - This root is the root file system for this process.
   - We can find the directory structure inside the **safrotWebServer** container, including the newly created file **safrot_new_file**.

<a name="desc2"></a>
### Namespaces in the Linux Kernel.

##### Overview

<img src="assets/Types_of_Namespaces.png"><br>

<a name="desc3"></a>
##### A Practical Look at How Containerization Tools Utilize the Namespace Feature in the Isolation Process.

<img src="assets/NSDockerExploration.png"><br>

<a name="desc4"></a>
### A closer look at some of the types of namespaces.
1. [A Mount Namespace - ```mnt```](#mnt)
   - [Overview.](#overview-mnt)
   - [Practical Example.](#Practical-ex-mnt)
2. [A Process ID Namespace - ```PID```](#pid)
   - [Overview.](#overview-pid)
   - [Practical Example.](#Practical-ex-pid)
   - [The risks you may face when using a shared namespace for all processes.](#risks-shared-ns-for-all-proc)
3. [A Network namespace - ```net```](#net)
    - [Overview.](#overview-net)
    - [Isolating Containers: Network Independence on Hosts - Overview.](#Isolating-Containers)
    - [Dealing with network namespaces on the Linux host.](#network-namespaces-Linux)


<a name="mnt"></a>
#### A Mount Namespace - ```mnt```
<a name="overview-mnt"></a>
##### Overview

<img src="assets/mntns.png"><br>

<a name="Practical-ex-mnt"></a>
##### Practical Example.

- Let's consider a scenario where you want to run two instances of a web server on the same host.
- Without mount namespaces, you might face challenges if the web server needs to write to the same directory for logs and so on.
- Each instance can have its own mount namespace, so even if they write to the same path e.g., ```/var/www/logs```, the changes are isolated to their respective mount namespaces.
- Now, let's inspect the Mount Namespace setup when running a container via **Process ID**:

<img src="assets/mntns1.png"><br>

> [!TIP]
> • As we proved before, the container is just a process. So, you can use this command ```sudo cat /proc/12147/mountinfo``` to see the Mount Namespace setup when running a container.<br>
> • **12147** is the process ID for my container; you can obtain your own by: ```docker inspect -f '{{.State.Pid}}' "Container Name"```<br>
> • We can interact directly with Linux namespaces by using the ```nsenter``` program.<br>
> • Let's apply it to our containers to see the file system of our **konohaWebserver** container.<br>
>```
>Killua@pop-os:~$ sudo nsenter --target 12147 --mount ls /
>bin  boot  dev	docker-entrypoint.d  docker-entrypoint.sh  etc	home  lib  lib32  lib64  libx32  media	mnt  opt  proc	root  run  sbin  srv  sys  tmp	usr  var
>```

<a name="pid"></a>
#### A Process ID Namespace - ```PID```

<a name="overview-pid"></a>
##### Overview

<img src="assets/pidns.png"><br>

<a name="Practical-ex-pid"></a>
##### Practical Example.

<img src="assets/pidns1.png"><br>

- _**sudo unshare --fork --pid --mount-proc /bin/bash**_
  - ```unshare```: Allows you to run a program with certain namespaces unshared from the parent process. In this case, it's creating a new PID namespace.
  - ```--fork```: This option tells unshare to fork the current shell into a new process before executing the specified command (```/bin/bash```). The forked process will be the one running in the new PID namespace.
  - ```--pid```:  creates a new PID namespace.
  - ```--mount-proc```: mounts a new /proc filesystem in that namespace.
  - The new shell is started with ```/bin/bash```.
- _**echo "Current shell PID: $$**_
  - prints the PID of the current shell in the new namespace. In this case, it shows that the PID is 1, indicating that it's the first process in the new PID namespace.
- _**ps aux**_
  - lists all processes in the current namespace. In this example, it shows that there are two processes: the **/bin/bash process (PID 1)** and the **ps aux command itself (PID 9)**.
- _**exit**_
  - exits the new shell and returns to the original namespace.
- _**echo "current shell pid: $$**_
  - This command, executed in the original shell, prints the PID of the original shell, which is ```13084``` in this case.

<p align="center">
<samp>
  In summary, the unshare command is used to create a new shell in a separate PID namespace. Within that namespace, the PID of the new shell is 1, and any processes started within that shell have PIDs isolated from the rest of the system. Exiting the new shell returns to the original namespace, where the PID of the original shell is different.</samp>
</p>

<a name="risks-shared-ns-for-all-proc"></a>
##### The risks you may face when using a shared namespace for all processes.

<img src="assets/risks-shared-ns-for-all-proc.png"><br>

- Depending on timing, the displayed content by ```cat``` in Terminal-1 may be the original or the new content written by Process 2.
- This scenario highlights potential interference between two processes in the same PID namespace, sharing access to a file, leading to unexpected outcomes.
- Utilizing separate PID namespaces is recommended to achieve process isolation and prevent interference issues in scenarios with shared resources.

<a name="net"></a>
#### A Network namespace - ```net```
<a name="overview-net"></a>
##### Overview

<img src="assets/overview-net.png"><br>

<a name="Isolating-Containers"></a>
##### Isolating Containers: Network Independence on Hosts - Overview.
<img src="assets/Isolating-Containers.png"><br>


<a name="network-namespaces-Linux"></a>
##### Dealing with network namespaces on the Linux host.

