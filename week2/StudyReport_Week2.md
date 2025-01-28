[toc]
# 2025大一上寒假学习报告
---
## 第二周

### 模拟电子技术基础学习
本周首先用书本重新巩固了之前学习的内容。

本周网课学习了7-10小节。
本周的重点内容如下：
~~~
BJT共射特性曲线
MOS管工作原理和性质
  - 绝缘栅型
  - 结型
基本放大电路的构成
~~~

### 线性代数网课学习
本周学习了1.7-2.3小节
本周重点内容如下：
~~~
行列式的计算
    -化上三角形行列式
    -爪形行列式
    -范德蒙德行列式

矩阵的概念
矩阵的运算
    -矩阵的加法、减法、数乘
    -矩阵的乘法及其性质

~~~

---
### STM32学习
#### HAL库开发环境搭建
**[keil + vscode + cubemx开发环境搭建教程](https://www.bilibili.com/video/BV19V411g7gD/)**

**工程搭建**
先用CubeMX初始化，注意在Project Manager选项卡中将Toolchain/IDE一项改成MDK-ARM，保存工程到指定文件夹。
用vscode打开MDK-ARM文件夹中的keil文件即可。

**关于下载后需要按reset键的问题**
[解决方法](https://blog.csdn.net/cql2439428362/article/details/141161144)
注意改完设置之后要重新生成一遍工程并且重新插一遍STLink才能生效！！！

**CUBEMX固件库获取**
[教程](https://blog.csdn.net/MP13537560060/article/details/140952278)
[ST官网下载链接](https://www.st.com.cn/zh/embedded-software/stm32cube-mcu-mpu-packages/products.html)

#### HAL库开发学习
****注意！***
```c
 /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    //代码写在这里才不会被mx刷没！！
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
```

---

##### 按键控制小灯
~~~c
/* Infinite loop */
 if (HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_11) == GPIO_PIN_SET) {
      HAL_Delay(20);
      HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_2);
      while(HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_11) == GPIO_PIN_SET);
      HAL_Delay(20);
    }
~~~
用Delay消振，增强稳定性。
硬件方法：并联电容

##### USART串口通信
**配置UART串口**
对于USART1，PA9代表STM32的TX，PA10代表RX，**注意TX连接收口的RX，RX连接收口的TX！**

  mx中要设置为异步模式(Asynchronous)

  还有一条线要共地(GND)

  **波特率**：每秒传输多少bit
  每传送一字节需要10bit（8位加1位起始位和1位停止位）

**串口调试助手**
  [波特律动串口助手](https://serial.keysking.com/)


**轮询模式**
发送信息：
~~~c
HAL_UART_Transmit(&huartx, [*uint8_t], <信息长度>, <最长等待时间>);
~~~
&huartx:&huart1/&huart2...
<信息长度>:可以使用strlen()取字符串长度

注意：如果是char xxx [ ]要使用类型强制转换为uint8_t xxx [ ]

---

接受信息：
~~~c
HAL_UART_Receive(&huartx, [*uint8_t], <信息长度>, HAL_MAX_DELAY);
~~~
HAL_MAX_DELAY：不限制等待时间

*注意：在旧版本中（如固件库版本1.8.4）由于该函数中没有清除RXEN标志位，需要在其前面使用手动清除：*
~~~c
__HAL_UART_CLEAR_FLAG(&huart1, UART_FLAG_RXNE);
~~~
[参考方案](https://blog.csdn.net/weixin_44536527/article/details/126503818)
****注：在更新了cubemx（v6.13.0）和固件库版本（目前版本1.8.6）后，上述解决办法已经不需要使用。***

---
**中断模式**
Cubemx中的配置：
除了常规的配置USART以外，还需要把NVIC interrupt Table 中的USARTx global interrupt 勾选 Enabled

生成工程后，打开stm32f1xx_it.c翻到最下面可以看到如下函数：
~~~c
void USART1_IRQHandler(void)
{
  /* USER CODE BEGIN USART1_IRQn 0 */

  /* USER CODE END USART1_IRQn 0 */
  HAL_UART_IRQHandler(&huart1);
  /* USER CODE BEGIN USART1_IRQn 1 */

  /* USER CODE END USART1_IRQn 1 */
}
~~~
对于其中的
HAL_UART_IRQHandler(&huart1);
跳转定义，往下翻找到其回调函数, 找到：
~~~c
__weak void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
{
  /* Prevent unused argument(s) compilation warning */
  UNUSED(huart);
  /* NOTE: This function should not be modified, when the callback is needed,
           the HAL_UART_RxCpltCallback could be implemented in the user file
   */
}
~~~
__weak 前缀表示弱定义，可以在别的地方重新定义该函数。
因此使用中断模式接受信息可以按如下调用：
~~~c
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart){
  
  /*接收信息后要运行的代码块*/

  HAL_UART_Receive_IT(&huart1, received, 2);//准备继续中断接收
}
~~~
**同时也要在main()函数中调用一次HAL_UART_Receive_IT()函数**（但不要在while循环中调用！）

另注：全局变量定义在

~~~
/* USER CODE BEGIN PV */

/* USER CODE END PV */
~~~
代码块中

---
**DMA模式**
Cubemx中的配置：
在USART配置选项卡中的DMA Settings 为TX和RX分别添加一条DMA通道。

将中断接收/发送函数改为如下函数即可：
```c
HAL_UART_Receive_DMA(&huart1, received, 2);
HAL_UART_Transmit_DMA(&huart1, received, 2);
```
相比于中断模式的优势：
DMA传输完成中断，只在接收或发送时触发，更少消耗CPU资源。
***注意：中断模式和DMA模式不需要最大等待时间参数！***

---
**串口接收不定长信息**
接收函数改为如下：
```c
HAL_UARTEx_ReceiveToIdle_DMA(&huart1, received, sizeof(received));
```
回调函数改为如下；
```c
void HAL_UARTEx_RxEventCallback(UART_HandleTypeDef *huart, uint16_t Size){
  if (huart->Instance == USART1) {  //判断串口
    HAL_UART_Transmit_DMA(&huart1, received, Size);

    HAL_UARTEx_ReceiveToIdle_DMA(&huart1, received, sizeof(received));
    __HAL_DMA_DISABLE_IT(&hdma_usart1_rx, DMA_IT_HT);   //关闭数据长度过半中断（DMA模式下特有）
  }
}
```
如果不能正常回复，检查如下问题：
1.初始化顺序：DMA初始化要在UART之前。
MX_DMA_Init();
MX_USART1_UART_Init();
2.***NVIC interrupt Table 中的USARTx global interrupt 有没有勾选 Enabled***



---

### 学术技巧
#### VPN 虚拟专用网络
- 两大功能
    **加密**
    **转发**
  <br>
- 主流加密协议
  ShadowSocks/SS
  SSR
  (以上两种目前最主流,目前已经被掌控)
  VMess
  V2Ray
  XRay
  **Trojan**（目前完全无法被检测）
  <br>
- 平台和客户端
  **Clash**//For Windows and Mac and Andios
  **ShadowRockets**//For IOS （IOS改美区ID账号，APPstore搜,美元原价兑换:支付宝地点改到旧金山，然后搜Pockyt Shop，买礼品卡）
  **Williams**//For IOS （下载方式同上但是免费）
  路由器（老毛子/梅林）
  <br>
- 节点
[西部世界](https://sj2612.com/i/iv250122/P59G9u0)
银杏

购买完节点之后复制订阅链接到客户端内的URL导入处导入配置即可。
  <br>
- 出站模式
  **全局连接**（一切请求都会被代理）
  **规则判断**（国内的不会被代理，国外的会被代理）
  **直接连接**
  <br>
- 规则配置
<br>

**AI**
Chatgpt（IOS APP）
DeepSeek

**注册窍门**
[手机号注册SMS Activate](https://sms-activate.guru/cn)
[美国地址生成器](https://www.meiguodizhi.com/)

#### 概念
后端：Django
API接口：Deepseek

#### 项目建议
基于YOLO实现物体识别
搞通linux命令行 建议Ubuntu入手 ROS2
嵌入式工具箱（8051 STM32 树莓派）
阿里云天池大数据 数据分析
