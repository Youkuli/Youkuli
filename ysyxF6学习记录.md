# ysyxF6学习记录

![image-20251007111046925](C:\Users\Sorab\AppData\Roaming\Typora\typora-user-images\image-20251007111046925.png)

![](C:\Users\Sorab\AppData\Roaming\Typora\typora-user-images\image-20251007111108688.png)



# `addi、jalr`指令完成电路记录

![image-20251010000808950](C:\Users\Sorab\AppData\Roaming\Typora\typora-user-images\image-20251010000808950.png)

![image-20251010002327211](C:\Users\Sorab\AppData\Roaming\Typora\typora-user-images\image-20251010002327211.png)

## 电路初值：

1. 计数器置位端上`0`下`1`,表示正常计数
2. `PC`加法器，正常循环一次+`1`
3. `PC`寄存器写使能
4. 读指令状态选择器[`mux`]，时序控制，jalr指令仅在读指令状态11时写使能D触发器，将下一条指令的`PC`值输出到寄存器`Data`线
5. 指令寄存器写使能，读指令状态机，使能信号源恒`1`
6. `jalr`指令防冲突设计：`comparator`两个计数器与`1011`，消除`GPR`写使能信号误触发
7. `addi`指令码译码
8. `jalr`指令码译码
9. `jalr`指令PC暂存寄存器
10. `jalr`指令PC+4的具体实现
11. `jalr`指令中对目标指令地址最低位强制置`0`
    - `risc-v`规范要求
12. 读取当前`PC`值并减`3`，得到指令首位`PC`值并送出