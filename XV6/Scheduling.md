
sched() is called **only** when a process wants to or forced to transit from running state to other state

different levels of time scale:
1. clock cycle
2. timer interrupt period
3. cpu burst

sleep(int tick) guarantee the process sleep at least n ticks, but not necessarily being waken up at the n-th ticks 
## How It works

Process being swapped out
yield() -> sched() -> swtch()
* swtch() swap in scheduler()'s callee saved register and `ra`, so when swtch() returns, it goes to scheduler()
