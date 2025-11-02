
Modern OS are interrupt driven

Purpose of interrupt: allowing a computer system to handle events (I/O, error, ...) without polling. This allow CPU to perform other tasks while waiting for an event

* Interrupt Service Routine (ISR)
	* What to do when receiving an interrupt
	* **Interrupt vector**: contain the address of the service routines

* Hardware interrupt (signal):
	* Asynchronous, can happen at any time
	* E.g. Printer signal CPU it finishes its job
* Software interrupt (trap): 
	* The root cause is the program itself, caused by a specific instruction, synchronous
	* E.g. error raised since divide by zero, user request OS service (system call)

