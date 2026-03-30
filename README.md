# 多週期 MIPS 架構 (Multicycle MIPS Architecture)

**[這份文件](./Multicycle_MIPS.pdf)**詳細解析了 **多週期 MIPS 架構 (Multicycle Architecture)**，並以圖解說明了處理器設計從單週期 (Single Cycle) 演進到多週期 (Multicycle) 的轉換過程。

## 執行階段 (Execution Stages)

多週期架構的核心機制是將指令的執行過程拆分成多個不同的階段 (Mechanism of Multicycle Architecture)。依據不同的指令，最多會經歷以下五個階段：
* **IF (Instruction Fetch)**：擷取指令。
* **ID (Instruction Decode)**：指令解碼與讀取暫存器。
* **EX (Execution)**：利用 ALU 進行執行運算或記憶體位址計算。
* **MEM (Memory Access)**：存取資料記憶體。
* **WB (Write Back)**：將結果寫回暫存器。

## 支援的指令範例與分析

文件中針對幾種經典的 MIPS 指令，詳細展示了其經過的資料路徑 (Datapath) 以及所需的執行階段：
* **R-Type 運算**：`add` 指令 (例如：`add $t2, $t0, $t1`)。
* **I-Type 運算**：`addi` 指令 (例如：`addi $t2, $t0, 10`)。
* **分支指令 (Branch)**：`beq` 指令 (例如：`beq $t1, $t0, 10`)。
* **記憶體存取指令**：
    * 載入字組 `lw` (例如：`lw $t1, 10($t0)`)。
    * 儲存字組 `sw` (例如：`sw $t1, 10($t0)`)。
* **跳躍指令 (Jump)**：`j` 指令 (例如：`j 1000`)。

## 控制單元與有限狀態機 (FSM)

多週期控制單元是透過有限狀態機 (FSM) 來驅動的。
* **狀態轉移**：FSM 涵蓋了從 **S0 到 S11** 的狀態，不同的指令 (如 lw, sw, R-type, beq, j, addi) 會根據其操作碼 (opcode) 走向不同的狀態分支。
* **控制訊號表 (State definition)**：文件中提供了一張完整的真值表，定義了在 S0 到 S11 每個狀態下的所有控制訊號值。這些控制訊號包含：`PCWriteCond`、`PCWrite`、`lorD`、`MemRead`、`MemWrite`、`MemtoReg`、`IRWrite`、`PCSource`、`ALUOp`、`ALUSrcB`、`ALUSrcA`、`RegWrite` 以及 `RegDst`。
