# Address Binding

Turning symbolic address (variable name) into physical address on memory

## Compile Time
- Absolute code
- Must be loaded into the exact location
- Logical address = physical address

![[Pasted image 20251102190831.png]]

## Load Time
- Relocatable code
- Use base address
- Logical address = physical address

![[Pasted image 20251102190903.png]]

## Execution Time
- Logical-address code
- ==Virtual memory address== must exist to support this
- Memory Management Unit (MMU)
	- Hardware that map logical address into physical address
	- ==Address from CPU is always logical address==
- Logical address $\neq$ physical address

![[Pasted image 20251102190958.png]]


# Linking & Loading

- Linking: Combine multiple object code into an executable
- Loading: Move executable file to memory

## Dynamic Loading
- Don't need to load the entire program into the memory when executing (e.g. error handling code)
- Better memory space utilization
- OS provide the service, but user needs to explicitly use it
	- `dlopen()`, `dlsym()`, `dlclose()`

## Static Linking
- Combine library code into the executable at compile time
- Static linking + dynamic loading still can't prevent duplicate code (multiple programs use the same library)

## Dynamic Linking
- Only one code copy in memory and shared by everyone
- A stub in included in the program to help locating the library
- Dynamically linked library may be loaded at different time
	- Load-Time dynamic linking: library is loaded at load time
	- Run-Time dynamic linking: using `dlopen()`, library is loaded at runtime

# Swapping

- A process can be swapped out to a backing store
- **Backing store**: A chunk of disk, ==separated from file system== 
- Swapping back
	- Compile/load time address binding: must be load the the same address
	- Execution time binding: load to anywhere
- ==A process to be swapped must be idle==
	- If a process is doing I/O with DMA, it can't be swapped out
	- Solution: don't swap a process with pending I/O
	- Solution: I/O operation are done through OS buffer
	> If no DMA, I/O are done through OS buffer, hence can be swapped

# Continuous Memory Allocation

- Fixed-Partition allocation
	- Each process loads into ==one partition of fixed-size==
	- Degree of multi-programming is bounded by the number of partition 
- Variable-Size partition
	- Continuous free memory (hole) of various size will be scattered in memory
	- ![[Pasted image 20251102205342.png]]
	- How to allocation
		- First-fit: allocate the 1st hole that fits
		- Best-fit: allocate the smallest hole that fits
		- Worst-fit: allocate the biggest hole that fits

Fragmentation
- External fragmentation
	- Enough memory space but not continuous, hence unusable
- Internal fragmentation
	- Memory inside a partition but not used
- Solution: ==Compaction==
	- Move memory content to make a big chunk of continuous free memory
	- Only feasible when binding is done at execution time, though impractical due to low efficiency
![[Pasted image 20251102205651.png]]

# Non-Continuous Memory Allocation

## Paging
- Divide **physical memory** into **fixed size** blocks called **frames**
- Divide **logical address space** into blocks of the same size called **pages**
- Allow physical address space of a process to be non-continuous
- No external fragmentation, limited internal fragmentation
- Provide shared pages
	- Shared code must be **reentrant code**, never change during execution
- ==Each process has its own page table, and been managed by OS==
- Total number of pages does not need to be the same as the total number of frame
	- Only the ==number of bits for page offset should be the same== for logical address and physical address
- ==Page table stored in OS memory, no in MMU==
- Larger page size
	- More internal fragmentation
	- Fewer page fault
	- Smaller page table

### Implementation of Page Table
- Page tables are kept in memory
- Page-table base register  (PTBR)
	- Physical memory address of the page table
	- Stored in PCB
	- Change PT when context switch $\rightarrow$ change value of PTBR
	- With page table, one memory reference needs 2 memory reads
		- First, access the PTE to get the physical page number
		- Second, physical page number + page offset to get the needed memory content
		- Solved by Translation Look-aside Buffer (TLB), ==caching the translation between VPN and PPN==
- TLB
	- A cache for page table ==shared by all processes==
	- TLB ==must be flushed when context switch==, or has a PID field
- Memory protection
	- Valid-Invalid bit
		- Valid: the page is in the process's logical address space
		- Valid page doesn't mean valid memory access
- Page table can be large
	- Needs to break into smaller ones, within a single page size

#### Hierarchical Paging
- Break up the **logical address space** into multiple page tables
- More level $\rightarrow$ More memory access

![[Screenshot_20251102_215203_Samsung Notes.jpg]]

- Different page size

![[Pasted image 20251102224145.png]]

#### Hash Page Table
- Commonly-used for address > 32bit
- Virtual page number is hashed
	- Each entry in the hash table contains
		1. Virtual page number
		2. Frame number 
		3. Next pointer
	- Pointer waste memory, traverse list cause additional memory references
- ==Suitable for large system with large address space==
	- Memory used for storing the page table is associated with used mapping, not the range of the address space (in contrast of hierarchical page table)
- Improved hashed page table
	- Map a group of PTE at once
	- ![[Pasted image 20251102221730.png]]

#### Inverted Page Table
- Maintain a **frame table**, each entry store
	- ==PID==
	- Page number
- Each access needs to search the whole frame table
- Hard to support shared page

## Segmentation
- A program is a collection of segments
- Logical address: ==(segment number, offset)==
	- ==Offset is the same as physical memory size==
- Segmentation Table
	- Each entry has base and limit
	- Base: the start of physical address
	- Limit: length of the segment, check offset length
- Segmentation Hardware
	- Segmentation table translate segment number to physical base address
	- Offset is compared with the limit, if violation $\rightarrow$ segmentation fault  
	- Physical address base and offset must be added, not concatenated, since lower bits of base address is not guaranteed to be all zero 
- Easy to do dynamic loading
	- One routine one segment
	- Paging: one routine may span across multiple pages
- Variable size is hard to allocate

## Segmentation with Paging
- Segmentation: user friendly
- Paging: OS friendly 
- Apply segmentation in logical address space, paging in physical address space
- Address Translation
	1. CPU generate logical address
	2. Given to **segmentation unit**, produce ==linear address==
	3. Linear address give to **paging unit**, produce physical address
	> Why called "Linear address": a segment is like a object in OOP, we linearize the object to a address
- Segmentation is done first, so linear address is always a valid address
	- Invalid memory access will lead to segmentation fault
	- Page fault $\rightarrow$ page not on memory
![[Pasted image 20251102224344.png]]