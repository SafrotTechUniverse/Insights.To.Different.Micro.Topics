# cgroups & Namespaces;

<img alt="meme.png" src="assets/meme.png" />


### Table of content

1. [What Are Namespaces? ```overview```.](#desc0)
2. [Types of Namespaces in Linux Kernel and Their Relationship with the Container Concept.](#desc1)

<a name="desc0"></a>
### What Are Namespaces? ```overview```.

<img alt="Why.png" src="assets/Why.png" />

##### So, let's say the technical term for the named space is:- 

- **Namespaces** are a feature of the Linux kernel that partitions kernel resources. This allows one set of processes to see a distinct set of resources, while another set of processes see a different set of resources.

> [!TIP]
> So, since the key feature of namespaces is to isolate processes from each other, this takes us to the container term.<br>
> Using containers in development gives us an isolated environment  that acts like a virtual machine. However, it's not a virtual machine; it's just a process running on a server. <br>
> Take a look [here](https://github.com/Mohamed-abdalazez/DockerInDeep#crucial-concept-between-a-virtual-machine-and-a-container) for a crucial concept comparing a virtual machine and a container.

<a name="desc1"></a>
### Types of Namespaces in Linux Kernel and Their Relationship with the Container Concept
##### In the Linux kernel, there exist various types of namespaces, each having its own unique properties:
- **user** namespace.
- **process ID (PID)** namespace.
- **network** namespace.
- **mount (mnt)** namespace.
- **interprocess communication (IPC)** namespace.
-  **UNIX Time‑Sharing (UTS)** namespace.

<p align="center">
   <img src="assets/Containers _Are_Just_Processes.png"  width="400px" height="450px"><br>
  <samp>
    So, let's check out their thing together🐬.
  </samp>
</p>



