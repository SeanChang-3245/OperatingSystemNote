
* Each CPU core has its own timer. (CLINT, Core Local Interruptor), but aging is ticks

First software code entered is `kernelvec.S/timervec`, after a timer signaling time's up.
Before entering `timervec`, machine-mode is activated (==HOW!!!!!==[1]). Inside the `timervec`, it set `SIP` to notify supervisor-mode that it needs to trigger a software interrupt after returning from machine-mode (==WHAT DECIDE IT NEED TO GO TO S-MODE!!![2]) 

1. Timer signals a Machine Timer Interrupt (MTIP), the hardware will switch to machine mode and jump to `mtvec`
2. `MPP` write to supervisor mode in `start`, and will never be changed

## Initializing Timer

是因為收到MTIP的時候會進入machine mode所以才會跳到 mtvec對嗎?如果硬體上的設計是跳到supervisor mode的話就是跳到stvec

是的，重點在 RISC‑V 的陷阱路由規則：

- 進哪個向量取決於「當前特權級」與「委派寄存器」：
  - 機器計時器中斷 MTIP 屬於 M‑mode 中斷，不能透過 mideleg 委派給 S‑mode，所以必定跳到 mtvec。
  - xv6 把 mtvec 指到 timervec；timervec 設定 sip 的 SSIP（或 STIP）交給 S‑mode，並排下一次計時器。
  - S‑mode 隨後因 SSIP/STIP 進入 stvec（kernelvec）。

- 只有「S 級的中斷/例外」（如 SSIP/STIP/SEIP，或被 M‑mode 用 medeleg/mideleg 委派的例外/中斷）才會直接跳到 stvec。

- 若硬體平台直接提供 S‑mode 計時器（STIP），那麼 S‑mode 就會直接進 stvec；CLINT 一般只產生 MTIP，因此要先經過 M‑mode 的 mtvec 再轉給 S‑mode。

## Timer Interrupt in User Space

## Timer Interrupt in Kernel Space
