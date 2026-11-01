# 实验 08 · 定时器定时中断

## 遇到的问题

`Num` 涨得比 1 秒慢很多（大约 9 秒才 +1）。

**检查**

1. 在 `main` 里显示实际主频：
   ```c
   RCC_ClocksTypeDef clk;
   RCC_GetClocksFreq(&clk);
   OLED_ShowNum(2, 1, clk.SYSCLK_Frequency / 1000000, 2);
   ```
2. 看显示的是 **72** 还是 **8**
3. 检查 PSC / ARR 的取值

**原因**

1. **板上 8 MHz 晶振没起振**（虚焊/缺件），`SystemInit()` 退回内部 8 MHz，
   定时器正好慢 9 倍
2. 临时补救：`TIM_Prescaler` 从 `7200-1` 改成 `800-1`，仍然 1 Hz
3. 另一个坑：`uint16_t Num` 超过 65535 会归零，要更大就改 `uint32_t`