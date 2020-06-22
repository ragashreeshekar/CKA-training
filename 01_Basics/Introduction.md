## What is Docker ? 
**Docker is a Containerization Platform which allows us to containerize an application/software (called as Docker Image) & 
also lets you run Containerized application/software**
	
**Technical Deffinition:**

Docker is an open source software platform to create, deploy and manage virtualized application containers 
on a common operating system (OS), with an ecosystem of allied tools.

Docker is a set of platform as a service products that use OS-level virtualization 
to deliver software in packages called containers.

## Docker Image
- A package which consists of an application/software with all its dependencies to run, **Called as Docker Image**. It is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings
- Docker Image will have a base layer of minimal OS in it always, on top of OS layer we install software & its dependencies. 
- A Docker image is built up from a series of layers. Each layer represents an instruction that we run. 
- Docker Images are immutable and are are stored in a Docker registry such as registry.hub.docker.com

## Docker Container

A container is runtime instance of a Docker Image. A Container is the actual instantiation of the image just like how an object is an instantiation or an instance of a class.

## Advantages of Containers 

#### Physical Machine vs. Virtual Machines vs. Container


| Physical | Virtaul Machines | **Containers** |
| :-------- | :-------------- | :---------- |
| No virtualization | Hardware level virtualization | **OS level Vertualization** |
| Longest time for procuring & provisioning | Longer time for procuring & provisioning | **Minimal time for provisioning** |
| Huge Maintenance Cost | Huge Maintenance Cost | **No Maintenance Cost** |
| No Scalability | Scalability is Hard | **Easily Scalable** |
| Huge Resource Wastage | Better Resource Usage but Dynamic Allocation is NOT Possible | **Dynamic Resource Allocation is Possible** |
| Takes Longer Time to Initialize App (boot time) | Almost Same as Physical  | **Take Very Less Time to Initialize App (less boot time)** |
