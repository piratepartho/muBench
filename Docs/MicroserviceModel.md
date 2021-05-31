# Microservice Model

### Table of Content
* [Introduction](/README.md)
* [**Microservice Model**](/Docs/MicroserviceModel.md)
  * [Service Cell](/Docs/MicroserviceModel.md#Service-Cell)
  * [Internal Service](/Docs/MicroserviceModel.md#Internal-Service)
  * [External Services](/Docs/MicroserviceModel.md#External-Services)
  * [Custom Functions](/Docs/MicroserviceModel.md#Custom-Functions)
* [Building Blocks](/Docs/BuildingBlocks.md)
  * [Service Mesh Generator](/Docs/BuildingBlocks.md#Service-Mesh-Generator)
  * [Work Model Generator](/Docs/BuildingBlocks.md#Work-Model-Generator)
  * [Workload Generator](/Docs/BuildingBlocks.md#Workload-Generator)
  * [Deployment](/Docs/BuildingBlocks.md#Deployment)
    * [Kubernetes](/Docs/BuildingBlocks.md#Kubernetes)
    * [Further Works](/Docs/BuildingBlocks.md#Further-Works)
* [Getting Started](/Docs/GettingStarted.md)
    * [Example](/Docs/GettingStarted.md#Example) - A step by step walkthrough
    * [Autopilot](/Docs/GettingStarted.md#AutoPilot) - The lazy shortcut

---
## Service Cell
Upon a service request, each service locally executes an **internal-service** and then carries out a set of calls towards **external-services**. An internal-service is a task that user can define as a python function to be inserted in the `/mnt/MSSharedData/JobFunctions` (see also [here](/Docs/MicroserviceModel.md#Custom-Functions)). However, each service has a pre-defined internal-service that is named `compute_pi`.

External services are grouped into a configurable number of groups (`service_groups`). Services from different groups are called in parallel; services from the same group are called sequentially. To mimic random paths on the service mesh, not all external services of a `service_group` are called, but only a subset of them, whose number is `seq_len` and these are chosen randomly (uniform distribution) from those in the `service_group`. 

---
## Internal Service

---
## External Services

---
## Custom Functions

Each service of the microservice mesh executes an internal-service when called and by default it uses the `compute_pi` function. 
The default function keeps the CPU busy depending on the specified complexity of operations.

To try other scenarios, you can use your own specific functions to stress the aspect you whish to simulate: CPU, memory or storage. 
In order to do so, you must write your own python function and save it to the subfolder `InternalServiceFunctions` inside your NFS shared directory.
If you followed our [NFS configuration](/Docs/NFSConfig.md), create the subfolder into `/mnt/MSSharedData` using 
`mkdir /mnt/MSSharedData/InternalServiceFunctions`, otherwise create it according to your NFS configurations.

### How to write your own custom job

- As input, your function receives a dictionary with the parameters specified in the [work model generator](/WorkModelGenerator/README.md).
- As output, your function must return a string used as body for the response given back by a service.

> Note: each custom function must have a unique name, otherwise conflicts will occur.
Also, you can specify more than one custom function inside the same python file.

```python
def custom_function(params):
    
    ## your code here

    ## the response of the function must be a string
    response_body = "the body must be a string"

    return response_body
```