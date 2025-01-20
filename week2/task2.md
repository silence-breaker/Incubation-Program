# ~~TASK2-TODO~~
## PID 相关

搭建一个电机小 demo

准备材料
- 一个磁编码器或者光电编码器的直流有刷电机
- 一块驱动板，比如 L298N 驱动板

前置知识
- 让 MCU 硬件外设产生 PWM 波形即可
- 利用 MCU 硬件外设使用定时中断采集编码器数据
- 用串口助手使用串口通信显示系统中各个量的值

完成的目标
1. 先让电机开环转起来
  - L298N 电源解决
  - 驱动正反转（满速）
  - PWM 输入进去
2. 搭建串口通信桥梁
  - 按照自定义的协议从上位机发送数据
	  - [＜PID调参＞VOFA+实现实时PID调参 (附源码)stm32_coward__123-GitCode 开源社区 (csdn.net)](https://gitcode.csdn.net/6627646316ca5020cb589c0f.html?dp_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MzgxNjQ0MSwiZXhwIjoxNzI0MjMwMzM2LCJpYXQiOjE3MjM2MjU1MzYsInVzZXJuYW1lIjoiWVl1ZV8ifQ.n9yCyH3C-MWA2fG4kDGbu-rUqfqZaC3P5l1b3sdtLr0)
  - 解析自定义协议的数据包
  - 分配数据到程序中的参数
  - 把程序中的数据按照上位机规定的协议发送出去
3. 搭建 PID 控制器
4. 参数整定
