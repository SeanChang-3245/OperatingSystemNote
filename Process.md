## Process Concept
* Program: Binary stored in disk
* Process: A program in execution in memory
* A **process** includes
	* Code segment
	* Data section: global variables
	* Stack: local variables
	* Heap: dynamic allocated variables
	* Current activity: PC, register value
	* OS resources: e.g. open file handlers
* Out of memory: run out of memory given by the OS, ==not physical memory==

* Thread
	* ==Basic unit of CPU utilization==
	* All threads belong to the same process
	* Share code segment, data section, heap and OS resources
	* Independent thread ID, PC, register set and stack

* Process State
	* New
	* Ready
	*  Running
	* Waiting
	* Terminated
![[Pasted image 20251102134934.png]]

* Context Switch
	* Purely overhead
	* Switch times depends on
		* Memory speed
		* Number of register
		* Existence of special instruction (single instruction to load/store all register)
		* Hardware support (multiple sets of register, change register file pointer means context switch)

## Process Scheduling
* ==Job queue== (new state): 
* Ready queue (ready state): 
* Device queue (wait state): 

### Schedulers
* Short-term Scheduler
	* [[Scheduling|CPU Scheduler]]
	* Which process should be executed
	* Ready $\rightarrow$ running
* Long-term Scheduler
	* Job scheduler
	* Which process should be loaded into memory
	* New $\rightarrow$ ready
	* Control **degree of multi-programming**
	* Select a good mix of CPU-bound and I/O-bound processes
	* Not often in modern system, use virtual memory instead
		> Batch system NEED job scheduler
* Medium-term scheduler
	* ==Swapper==
	* Which process should be swapped in/out memory
	* Ready $\leftrightarrow$ wait
	* ==Swap unit: process $\leftrightarrow$ virtual memory: page==
	* Not often in modern system, use virtual memory instead (or having enough physical memory)

## Operations on Process
- Process Tree: All processes resides in the same tree
- Process creation on Linux/Unix
	- `fork()`
		- Child duplicate the address space of the parent
		- Child and parent executes concurrently
		- Return value for child is 0, for parent is the child PID
		- **Copy-on-write** to save memory
	- `execlp()`
		- Load new binary file into memory, destroy old code
	- `wait()`
		- Parent wait on one of its child
- Process termination
	- Parent may terminate it s child by specifying its PID (`abort()`)
	- Cascading termination: parent exit $\rightarrow$ child exit

## Inter-process Communication

Requirements:
- Independent process cannot affect others
- Cooperating process, the other way

### Shared Memory
- Implemented by memory access
- Need user synchronization

### Message Passing
- Implemented by system call

- Direct Communication
	- Processes must name each other explicitly
	- ==One-to-one== relationship between links and processes
	- Links are automatically created
	- Limited modularity: if the name of a process is changed, program should be changed
- Indirect Communication
	- Messages are directed and received from ==mailbox==
	- Process send to/receive from the mailbox
	- Process can communicate if they **share a mailbox**
	- Many-to-many relationship between links and processes
	- Who to receive the message?
		- Select arbitrarily, and notify the sender who received
		- Allow only one process at a time to do receive operation 

Buffer Implementation
- Zero capacity: blocking send/receive
- Bounded capacity: if full, sender will be blocked
- Unbounded capacity: sender never block

### Sockets
- Network connection identified by IP and port
- Exchange unstructured stream of bytes
- Data parsing are user's responsibility

### Remote Procedure Call
- Cause a procedure to execute in another address space
- Abstract procedure call between processes on networked systems
- Parameter Marshalling: packs parameters into messages
- Problems
	- Small endian or big endian
	- Size of data type
	- 