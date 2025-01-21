# 1.21-1.28(20:00)
# ~~TASK2-TODO~~
## PID 相关(有时间的拓展任务)

搭建一个电机小 demo

准备材料
- 一个磁编码器或者光电编码器的直流有刷电机
- 一块驱动板，比如 L298N 驱动板

前置知识
- 让 MCU 硬件外设产生 PWM 波形即可
- 利用 MCU 硬件外设使用定时中断采集编码器数据
- 用串口助手使用串口通信显示系统中各个量的值，对应的波形图

完成的目标
1. 先让电机开环转起来
	- L298N 电源解决
  	- 驱动正反转（满速）
  	- 把 PWM 输入进去
2. 搭建串口通信桥梁
	- 按照自定义的协议从上位机发送数据
  		- 上位机安利 vofa+
		- [＜PID调参＞VOFA+实现实时PID调参 (附源码)stm32_coward__123-GitCode 开源社区 (csdn.net)](https://gitcode.csdn.net/6627646316ca5020cb589c0f.html?dp_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MzgxNjQ0MSwiZXhwIjoxNzI0MjMwMzM2LCJpYXQiOjE3MjM2MjU1MzYsInVzZXJuYW1lIjoiWVl1ZV8ifQ.n9yCyH3C-MWA2fG4kDGbu-rUqfqZaC3P5l1b3sdtLr0)
  	- 在程序中解析接收到来自上位机的自定义协议的数据包
  	- 然后把外来数据包分配数据到程序中 PID 的参数实现串口实时调参
  	- 把程序中的数据按照上位机规定的协议发送出去，显示波形
3. 搭建 PID 控制器
	- 参考 arduino PID 库作者的 blog 进行搭建，[打开这个链接](https://www.circlemoon.top/2025/01/21/robotics/AutoPilot-learning-record/Autopilot-learning-record/)，在里面有导航
 	- 或者参考 b 站教程视频也可以
4. 参数整定
	- 学习 Lambda 整定方法，推荐阅读
	- ![2e46c662719b635871caf6bc74ab4d2](https://github.com/user-attachments/assets/e4f232bc-f449-4d19-af6c-562713262aa2)


## 课内安排

- 所有人至少完成5节模电的学习，可以额外的饶有兴趣地学习线代和高数
- 学习STM32，搭配hal库，准备一些常用的库函数，用hal复现江科大中的标准库工程（进度比较难控制，大家尽可能努力吧！）
