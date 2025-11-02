# Historical Prospective

## Mainframe system

* Better reliability and security
* Used in hospital, banks

### Batch System
* One job at a time
* No iteration
* Low CPU utilization due to I/O -> Multi-Programming system

### Multi-Programming System
* I/O, computation overlap
* ==I/O must be done without CPU intervention==
* Several jobs are kept in memory
	* Job scheduling: which job to load to memory
	* CPU scheduling: which job to be executed
* Memory management: Allocate job for multiple tasks
* High CPU utilization but no user interaction -> Time-Sharing System

![[Pasted image 20251102135959.png]]

### Time-Sharing System
* Multiple users can share the computer simultaneously
* Virtual memory: Swap in and out jobs since physical memory is not large enough
* Synchronization

## Desktop System

* Dedicated to single user
* Transistor density $\neq$ performance -> Multi-core
## Parallel System

* Multi-processor system
* Symmetric Multi-Processor system (SMP)
	* Each processor runs the same OS
* Asymmetric Multi-Processor system (AMP)
	* One master CPU and slave CPUs
	* Extremely large system
* ==Memory Access Architecture==
	* UMA (Uniform Memory Access)
		*　Equal access time to memory
		* 		　![[Pasted image 20251101213923.png]]
	* NUMA
		* Often made by physically linking more SMPs (e.g. 2-socket server)
		* Memory access across link is slower
		* ![[Pasted image 20251101214001.png]]
* Multi-core processor: multiple cores on the same chip
* Multi-processor
## Distributed System

* Purpose
	* Resource sharing
	* Load sharing

> Unified Coherent Memory
> Prevent memory copy
> ![[Pasted image 20251101220228.png]]

## Real-Time Operating System

* ==Keeping deadlines==
* Control device in dedicated application
* Guaranteed response times
	* Hard Real Time: missing deadline results in a fundamental failure
	* Soft Real Time: missing deadline is unwanted, but not immediately critical
![[Pasted image 20251101223334.png]]

# Introduction

* Computer system 
	* User
	* Application
	* OS: ==Control and coordinates the use of the hardware==
	* Hardware
* ==Purpose of Operating System==
	* Resource Management
	* Abstraction
	* Protection
* ![[Pasted image 20251101225918.png]]
* ==System API is the only interface between user application and hardware==

* Computer-System Operations
	* One device controller <-> a particular device type
	* CPU only talks to the controller, not the device (mitigate I/O bound)
	* How CPU know the device finishes I/O
		* Busy wait: keep asking the controller (busy waiting)
		* [[Interrupt]]: controller notify CPU when finish
