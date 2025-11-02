# Demand Paging
- A page rather than the whole process is brought into memory
	- Faster process creation
	- Less memory needed $\rightarrow$ more users
- Page is needed when there is a reference to the page
	- Invalid reference $\rightarrow$ abort (checked by segmentation)
	- Page not in memory $\rightarrow$ page fault
	> Segmentation fault $\rightarrow$ handled by programmer
	> Page fault $\rightarrow$ handled by OS
- **Pure Demand Paging**
	- Start a process with no page, including pages for instruction
	- Never bring a page into memory until needed
- Swapper & **Pager**
	- [[Process#Schedulers|Swapper]] manipulate entire process
	- Pager manipulate individual pages
- Hardware support: Valid-Invalid bit
	- Valid: page is in memory
	- Invalid: page is not in memory
- Page Fault
	- MMU signal OS there is a page fault (==an interrupt==)
	- OS needs to get a free frame, swap in the page and ==restart instruction==
	- Require I/O operation
- Performance of paging
	- Access time is proportional to page fault rate
	- Larger page $\rightarrow$ less page fault

## Replacement Algorithm
- When no free frame, we have to find a **victim** to be swapped back to disk
- Use dirty bit to reduce overhead
	- If dirty, swapped out page needs to be write back to disk
	- If not dirty, can be freed directly
- Goal: lowest page fault rate

### First-In-First-Out Algorithm
- ==Belady's Anomaly==
	- More pages may cause more page fault
	- Hard to do resource management

### Belady Algorithm
- ==Optimal== algorithm
- Replace the page that ==will== not be used for the longest period of time
	- Need future knowledge

### LRU Algorithm
- Approximation of optimal algorithm
- Replace the page that ==has not== been used for the longest time
- Counter Implementation
	- Each page has referenced time stamp
	- Require linear search and lots of space
- Stack Implementation
	- Move referenced page to the top of the double-linked list
	- Use hashing to get referencing page
- ==No Belady's anomaly==
	- LRU is a stack algorithm, and stack algorithm won't have Belady's anomaly 

### Stack Algorithm
- The set of pages in memory for n frames is always a subset of the set of pages that would be in memory with n+1 frames

### Counting Algorithms
- Not common, since expensive and not perform well
- Least Frequently Used Algorithm
	- Actively used page should have a large reference count
- Most Frequently Used Algorithm
	- Page with smallest count was probably just brought in

# Process Creation
- Copy-on-Write
	- Parent and child shared the same frame
	- Only copy a frame when either of them modified the frame
	- Free frames are allocated from a pool of ==zeroed-out frames== for security
	- Efficient process creation
- Memory-Mapped Files
	- Allow file I/O to be treated as memory access by ==mapping a disk block to a memory frame==
	- File is initially read through ==demand paging==, subsequent access are memory access
	- Write back
	> Write through, slow
	- Pros:
		- Faster
		- Allowing shared file
	- Cons:
		- Security (concurrent write)
		- Data lost (power down, RAM is volatile)

# Allocation of Frames
- Each process needs a minimum number of frames in order to be functional
	- ==Depends on instruction set==
	- ![[Pasted image 20251103000033.png]]

- Fixed Allocation
	- **Equal allocation**: 100 frames, 5 processes $\rightarrow$ 20 frames/process
	- **Proportional allocation**: allocation based on the size of the process
- Priority Allocation
	- Proportional allocation based on process priority
	- Replace one of its frame or ==frame from a process with lower priority==

- Local Allocation
	- Replacement is chosen from the process's own set of allocated frames 
- Global Allocation
	- Replacement is chosen from the the set of all frames 
	- Number of frame for a process might be changed
		- Minimum number of frames must be maintained for each process to prevent ==thrashing==

# Thrashing
- If a process doesn't have enough ==frame==, it needs high paging activity
- ==A process is thrashing if it spending more time paging than executing==
- ![[Pasted image 20251103000833.png]]
- Vicious cycle of thrashing
	1. Process queued for I/O to swap
	2. Low CPU utilization
	3. OS increase degree of multi-programming
	4. New process takes frames from old processes
	5. More page fault
	6. Lower CPU utilization
- Must provide enough frames for each process to prevent thrashing

## Working-Set Model
- Locality: a set of actively used page
- Locality model: as a process executes, it moves from locality to locality
- Working-set model
	- Working-set window: a parameter $\Delta$
	- Working set: set of pages in most recent $\Delta$ page reference
- $S_i$: working set of process $i$
- $D = \sum \Vert S_i\Vert$: total demand frames
- If available frames < $D$ $\rightarrow$ thrashing
	- Suspend process to prevent thrashing
- Too expensive

## Page Fault Frequency Scheme
- Measure and control page-fault rate
- Establish upper and ==lower bound== on the desired page fault rate ==of a process==
- Almost no additional overhead, since page fault will raise interrupt by its own, just record the value
- 
