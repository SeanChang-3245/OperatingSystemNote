
All processes form a process tree

after fork(), wait() (if called) will sleep on the process itself, once children process exited, it will wake up processes sleeping on its parent, so the original process which called wait() will now wake up and free the child process.

The reason why ZOMBIE state exists is that parent process may wait on this child process, and some information needs to pass to the parent when the child exited, for example `pid` and exit status. After parent being waken up and scheduled into running state, it will start to deallocate the child process.

Channel == waiting queue
waiting queue 
	1. process可以加到waiting queue
	2. 可以把在queue裡面的process放到ready queue