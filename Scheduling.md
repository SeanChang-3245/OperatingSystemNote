# Basic Concept
- Process execution consists of CPU burst and I/O burst (interleaved)
- Schedule decision is made when a process
	1. Switch from running to waiting
	2. Switch from running to ready
	3. Switch from waiting/new to ready
	4. Terminate
- Non-Preemptive
	- Schedule under 1. and 4.
	- Process keep executing until termination or switching to waiting state
	- Malicious process may occupy the CPU
- Preemptive
	- Scheduler under all cases
	- Inconsistent state of ==shared data==
		- Since running process could be changed at any time, and shared data manipulation may not be atomic
		- Needs [[synchronization]]

Dispatcher
- Scheduler: pick which process can execute
- Dispatcher: give control of the CPU to the process
	- Dispatch latency: from stopping one process to start another one

# Scheduling Algorithm

## Metrics
- CPU utilization
- Throughput: number of completed processes per time unit
- Turnaround time: submission to completion (user end-to-end time)
- Waiting time: total waiting time in the **ready queue**
- Response time: submission to first time entering running state

## First Come First Serve
- Non-preemptive
- Convoy effect (Head of line blocking): short processes behind a long process wait long

## Shortest-Job-First
- Can be preemptive or non-preemptive
- Preemptive
	- If a new process has shorter CPU burst, preemption happen
- Wait time = Completion time - arrival time - CPU burst time - I/O time
- ==Starvation==: Long CPU burst may not be executed

Approximate SJF
- No way to know the length of CPU burst
- Use exponential average to predict next burst length

## Priority 
- Can be preemptive or non-preemptive
- ==Starvation==
	- Solution: Aging

## Round-Robin
- Preemptive
- Preempt after exceeding time quantum
- TQ too small: context switch overhead
- TQ too large: become FCFS
- Longer turnaround time than SJF, but shorter response time

## Multilevel Queue 
- Ready queue is partitioned into separated queues, each queue has its own scheduling algorithm
- ==Scheduling between queues==
	1. Fixed priority scheduling: starvation, since ==process can't move between queues==
	2. Time slice: each queue gets a certain amount of CPU time

## Multilevel Feedback Queue
- ==Process can move between queues==, according to ==runtime behavior==
- ==High level queue must be executed first==, use aging to solve starvation


#  Multi-Processor Scheduling
- AMP
	- Scheduling handled by master
- SMP
	- Each process is self-scheduling, ready queue is shared
	- Needs Synchronization

Process Affinity
- Cache builds up on the running processor
	- $\Rightarrow$ switch processor = cache miss
- Soft affinity: possible to migrate between processors
- Hard affinity: not to migrate to other processor

NUMA and CPU scheduling
- Where to place the process matters
![[Pasted image 20251102171055.png]]

# Real-Time Scheduling
- ==Real-time does not mean speed, but keeping deadlines==
- FCFS is not real-time scheduling

## Rate Monotonic algorithm
- Shorter period $\rightarrow$ Higher priority
- Fixed-priority

## Earliest-Deadline-First
- Earlier deadline $\rightarrow$ Higher priority
- Dynamic priority