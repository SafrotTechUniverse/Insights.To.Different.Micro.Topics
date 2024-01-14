# cgroups & Namespaces;

<img alt="Why.png" src="assets/Why.png" />


### Table of content

1. [What Are Namespaces? ```overview```.](#desc0)

<a name="desc0"></a>
### What Are Namespaces? ```overview```.
<img alt="overview.png" src="assets/overview.png" />

##### So, let's say the technical term for the named space is:- 

- **Namespaces** are a feature of the Linux kernel that partitions kernel resources. This allows one set of processes to see a distinct set of resources, while another set of processes see a different set of resources.

> [!TIP]
> So, since the key feature of namespaces is to isolate processes from each other, this takes us to the container term.<br>
> Using containers in development gives us an isolated environment  that acts like a virtual machine. However, it's not a virtual machine; it's just a process running on a server. <br>
> Take a look [here](https://github.com/Mohamed-abdalazez/DockerInDeep#crucial-concept-between-a-virtual-machine-and-a-container) for a crucial concept comparing a virtual machine and a container.
