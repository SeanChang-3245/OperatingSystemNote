
# OS Service

* ==User interface==
* Program Execution
* I/O operations
* File-system manipulation
* ==Communication==
* Error detection
* Resource allocation
* Accounting
* Protection

## User Interface

* CLI (Command Line Interface)
	* Shell
	* ==A user space program==, between kernel and user, the command line interpreter
	* Can be modified and selectable, e.g. Bash, CShell
	* 
* GUI

## Communication

![[Pasted image 20251102101356.png]]
* Message passing
* Shared memory
	* OS create shared memory
	* During communication, OS doesn't involve

# OS Application Interface

System call & API
![[Pasted image 20251102102847.png]]

* System call
	* The **OS interface** to a running program
	* How to pass parameter
		1. Pass in register
		2. Store in a table in memory, than pass the table address
		3. Push into stack
* API (Application Program Interface)
	* High level abstraction of system call
	* One API may use multiple system calls
	* Commonly ==implemented== by language libraries, e.g. C library
	* POSIX API for POSIX-based systems
		* Portable Operating System ==Interface== for Unix
		* Define what ==API== should be provided and its behavior, doesn't care about what system call to use and how these APIs are implemented, since system calls differ on different system
		* POSIX API can be implemented by C library

## Question
* Why use API rather than system calls?
	> Simplicity
	> Portability

# OS Structure

## Simple OS Architecture
* Only one or two levels of code
* Unsafe, hard to enhance 

![[Pasted image 20251102123521.png]]

## Layered OS Architecture
* High level only depends on lower level's services
* Pros: Easy to maintain
* Cons: Hard to define layer, less efficient
	* E.g. virtual memory involve I/O and memory management

![[Pasted image 20251102123531.png]]

## Microkernel OS
* Minimize kernel code, moves as much from kernel to user space
* ==Communication is provided by message passing==, no shared memory
	* Message can be verified by the kernel
* Pros
	* Easier for extending and porting
		* No corresponding hardware $\rightarrow$ extract corresponding module
	* Smaller kernel $\rightarrow$ less bugs
* Cons
	* Less efficient due to message passing
* Module (server) is in user space

![[Pasted image 20251102123550.png]]

## Modular OS Architecture
* Module is in kernel space
	* Each is loadable as needed
	* Talks to other over known interfaces

![[Pasted image 20251102123606.png]]

* How is works?
	* Plug in mouse, result in a interrupt
	* Kernel looks into the product ID and search for which module to load
	* When the module is loaded, it requests to add its own ISR to the interrupt vector
	* When the mouse is moved, based on the hardware interrupt signal, the interrupt is served by the corresponding ISR

## Virtual Machine
* Provides an interface identical to the underlying hardware
* Difficult to achieve due to critical instruction
	* Both user and kernel can execute, but with different outcome
* Host OS has hypervisor kernel module
	1. Guest OS use privileged instruction
	2. Hardware raise exception
	3. Host OS catch exception
	4. Hypervisor redo the privileged instruction
* Pros
	* Protection
	* Solve compatibility issue
	* Used to develop OS

### Full Virtualization
* Guest OS don't need to be modified, and completed unaware of it has been run on hypervisor
* VMware, KVM

### Para-Virtualization
* Guest must be modified, use hypercall to directly talk to hypervisor
* Better performance