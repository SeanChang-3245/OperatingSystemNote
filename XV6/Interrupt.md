
這邊要下指令才開啟interruptable，所以當interrupt發生時，硬體會自動把interruptable關掉嗎

是。RISC‑V 在進入 trap 時，硬體會自動關掉當前特權級的中斷使能位：

- 進 S-mode trap：清 sstatus.SIE，並把原值存到 sstatus.SPIE。
- 進 M-mode trap：清 mstatus.MIE，並把原值存到 mstatus.MPIE。

因此 handler 一開始是「不可中斷」的。xv6 在 usertrap 先處理/保存 sepc、scause、sstatus 後，才呼叫 intr_on() 重新開 SIE，讓系統呼叫期間可被裝置中斷搶斷。返回時 sret/mret 會把 SIE/MIE 從 SPIE/MPIE 還原，實現「進入時關、返回時按先前狀態還原」
> handbook P.44 

- SIE：sstatus 的「Supervisor Interrupt Enable」。控制目前在 S-mode 時是否可接收中斷（全域開關）。intr_on()/intr_off 就是在設/清 SIE。
- SPIE：sstatus 的「保存的 SIE」。硬體用來在陷入/返回時自動保存與還原 SIE。

陷入與返回規則

- 進入 S-mode trap 時：SPIE ← SIE；SIE ← 0（進 handler 先關中斷）。
- sret 返回時：SIE ← SPIE；SPIE ← 1；SPP 依返回目標調整。

---

xv6 為何在 usertrapret 設 SPIE

- 先把 SPIE=1，sret 之後 SIE 會變成 1，表示「回到使用者後，中斷已啟用」，可立即被下一個計時器中斷搶占。
- 若不設，sret 後 SIE=0，使用者程式將不會被計時器中斷搶占。

---
不論什麼SIE為何，只要MTIP被拉起後，就會直接進入timervec對嗎

不是。「會進入 timervec」需要同時滿足：

- mstatus.MIE = 1（機器級全域中斷開啟）
- mie.MTIE = 1（啟用機器計時器中斷來源）
- MTIP 為 pending

SIE/SPIE 與是否進入 timervec 無關；它們只影響 S 級中斷。  
若 MIE=0 或 MTIE=0，MTIP 只會保持掛起，等之後打開再陷入 M 模式到 mtvec。
## Software Interrupt
* First executed code: `uservec` in supervisor mode

## Timer Interrupt
* First executed code: `timervec` in machine mode
* CPU hardware senses this interrupt by a hardware wire
## Device Interrupt
* First executed code: `kernelvec` in machine mode
* CPU hardware senses this interrupt by a hardware wire