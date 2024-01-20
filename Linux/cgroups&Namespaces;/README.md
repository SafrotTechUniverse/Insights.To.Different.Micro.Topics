# cgroups & Namespaces;

<img alt="meme.png" src="assets/meme.png" />


### Table of content

1. [What Are Namespaces? ```overview```.](#desc0)
2. [Proof that containers are just processes.](#desc1)
4. [Namespaces in the Linux Kernel.](#desc2)
5. [A Practical Look at How Containerization Tools Utilize the Namespace Feature in the Isolation Process.](#desc2)
<a name="desc0"></a>
### What Are Namespaces? ```overview```.

<img alt="Why.png" src="assets/Why.png" />

##### So, let's explore the technical definition for namespaces.s:- 

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

##### 1. Initial Check.
   - Ran ```ps``` command to check processes related to nginx.**_nothing._**
##### 2. Docker Container Creation.
   - Executed ```docker run``` command to create a Docker container named safrotWebServer running the nginx image in detached mode ```-d```.
##### 3. Check Nginx processes again and get the process ID for the Nginx master process.
##### 4. Access **safrotWebServer** Container.
   - Used ```docker exec -it``` to access the bash shell inside the **safrotWebServer** Container.
##### 5. Created a new file named safrot_new_file inside the container.
##### 6. Exit the Docker container, navigate to the Nginx process directory, which we just obtained in step 3.
##### 7. List Contents of Nginx Process Root Directory
   - This root is the root file system for this process.
   - We can find the directory structure inside the **safrotWebServer** container, including the newly created file **safrot_new_file**.

<a name="desc2"></a>
### Namespaces in the Linux Kernel.

<img src="assets/Types_of_Namespaces.png"><br>

<a name="desc3"></a>
#### A Practical Look at How Containerization Tools Utilize the Namespace Feature in the Isolation Process.

<img src="assets/NSDockerExploration.png"><br>
