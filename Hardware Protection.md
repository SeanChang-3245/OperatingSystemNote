Shared resource requires OS to ensure that an incorrect program cannot cause other programs to execute incorrectly

# Dual-Mode Operation

* **Hardware** must support at least two mode, user and kernel mode
* **Privileged Instruction**
	* Executed only in kernel mode
	* Can be requested by user through system call
# I/O protection
* ==All I/O are privileged instruction==, since I/O devices are shared resource

# Memory Protection
* Protect interrupt vector and ISR
* Protect data access from other program
* HW support base register and limit register

# CPU protection
* Prevent user from not returning control
* HW support timer

# Questions
* Some computer systems do not provide a privileged mode of operation in hardware. Is it possible to construct a secure operating system for these computer systems?
	> Possible: use a software-based protection, e.g. virtual machine, to manage all the instruction

	> Impossible: software always have some bug 