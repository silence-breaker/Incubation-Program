[toc]
# 2025大一上寒假学习报告
---
## 第二周

### 模拟电子技术基础学习
本周复习巩固之前学的内容。

### 线性代数网课学习
本周学习了2.3-2.9小节
本周重点内容如下：
~~~
方阵的幂
矩阵的转置
方阵的行列式
方阵的伴随矩阵
逆矩阵
~~~

---
### STM32学习
#### HAL库开发学习
##### 蓝牙模块串口透传
配置CUBEMX：
USART串口设置异步模式，**注意改波特率**比如我用的蓝牙模块默认波特率为9600。
开启中断，添加DMA通道（TX,RX都要）

代码使用：
还是使用```HAL_UARTEx_ReceiveToIdle_DMA(); ```函数接收不定长数据

注意：
回调函数中
```c
void HAL_UARTEx_RxEventCallback(UART_HandleTypeDef *huart, uint16_t Size){
  if (huart->Instance == USART1) {  //判断串口
    HAL_UART_Transmit_DMA(&huart1, received, Size);//这里的字节长度是Size，即实际长度

    HAL_UARTEx_ReceiveToIdle_DMA(&huart1, received, sizeof(received));//这里的字节长度应该设置为全长，而不是Size。
    __HAL_DMA_DISABLE_IT(&hdma_usart1_rx, DMA_IT_HT);   //关闭数据长度过半中断（DMA模式下特有）
  }
}
```
##### OLED
配置CUBEMX：
- 使能IIC1
  目前OLED已经接上IIC1的B6 B7引脚
- 设置为快速模式

驱动库：[波特律动·OLED驱动(STM32+HAL+SSD1306)](https://led.baud-dance.com/)

---

### DEEPSEEK初探
Deepseek蒸馏模型本地部署
huggingface下载镜像：（需要魔法）
[下载链接（以32B为例）](https://huggingface.co/bartowski/DeepSeek-R1-Distill-Qwen-32B-GGUF/tree/main)
xxB代表训练数据量，xxQ代表量化程度
据说Qwen底模效果不如llama底模
本地部署的模型效果远不如671B完整体，后面使用会考虑使用云服务器部署或者购买API。

目前使用[硅基流动](https://cloud.siliconflow.cn/i/zwKWX9Ug)专线，然后可以调用API到[Cherry-Studio](https://cherry-ai.com/)上方便日常使用。

---

### YOLO初探

[入门教程【2025最新YOLO目标检测训练/开发教程（Python 人工智能Ai视觉模型）】](https://www.bilibili.com/video/BV1XECzYLECa/?share_source=copy_web&vd_source=07343a22e791d6df8e8857b42ee60032)


下载ultralytics库（使用阿里云镜像源）
```sh
pip install ultralytics -i https://mirrors.aliyun.com/pypi/simple/
```

#### 视频切割
你可以使用 Python 的`OpenCV`库来实现将视频按每 5 秒一张图进行分割并导出图片。`OpenCV` 是一个强大的计算机视觉库，它提供了处理视频和图像的丰富功能。以下是具体的实现步骤和示例代码：

实现思路
1. **打开视频文件**：使用 `cv2.VideoCapture` 打开指定的视频文件。
2. **获取视频的帧率（FPS）和总帧数**：通过 `get` 方法获取视频的帧率和总帧数，用于后续计算。
3. **计算每 5 秒对应的帧数**：根据帧率计算出每 5 秒包含的帧数。
4. **逐帧读取视频**：使用 `read` 方法逐帧读取视频，每隔计算出的帧数保存一次图像。
5. **保存图像**：使用 `cv2.imwrite` 方法将图像保存到指定的文件夹中。

示例代码
```python
import cv2
import os

def split_video_into_frames(video_path, output_folder, interval=5):
    # 确保输出文件夹存在
    if not os.path.exists(output_folder):
        os.makedirs(output_folder)

    # 打开视频文件
    cap = cv2.VideoCapture(video_path)

    # 获取视频的帧率（FPS）
    fps = cap.get(cv2.CAP_PROP_FPS)

    # 计算每 5 秒对应的帧数
    frames_per_interval = int(fps * interval)

    frame_count = 0
    save_count = 0

    while True:
        ret, frame = cap.read()

        if not ret:
            break

        # 每隔 frames_per_interval 帧保存一次图像
        if frame_count % frames_per_interval == 0:
            image_name = f"frame_{save_count:04d}.jpg"
            image_path = os.path.join(output_folder, image_name)
            cv2.imwrite(image_path, frame)
            save_count += 1

        frame_count += 1

    # 释放视频捕获对象
    cap.release()
    print(f"共保存了 {save_count} 张图像到 {output_folder} 文件夹中。")

# 使用示例
video_path = "your_video.mp4"  # 替换为你的视频文件路径
output_folder = "output_frames"  # 替换为你想要保存图像的文件夹路径
split_video_into_frames(video_path, output_folder)
```

代码解释
1. **函数定义**：`split_video_into_frames` 函数接受三个参数：视频文件路径 `video_path`、输出文件夹路径 `output_folder` 和分割间隔 `interval`（默认为 5 秒）。
2. **检查输出文件夹**：使用 `os.path.exists` 检查输出文件夹是否存在，如果不存在则使用 `os.makedirs` 创建。
3. **打开视频文件**：使用 `cv2.VideoCapture` 打开视频文件。
4. **获取帧率**：使用 `cap.get(cv2.CAP_PROP_FPS)` 获取视频的帧率。
5. **计算帧数**：根据帧率计算每 5 秒对应的帧数。
6. **逐帧读取视频**：使用 `cap.read()` 逐帧读取视频，直到视频结束。
7. **保存图像**：每隔 `frames_per_interval` 帧保存一次图像，使用 `cv2.imwrite` 将图像保存到指定的文件夹中。
8. **释放资源**：使用 `cap.release()` 释放视频捕获对象。

注意事项
- 请将 `video_path` 替换为你实际的视频文件路径，将 `output_folder` 替换为你想要保存图像的文件夹路径。
- 确保你的系统已经安装了 `OpenCV` 库，可以使用 `pip install opencv-python` 进行安装。


#### 标注平台

【EasyData免费在线标注平台】 [EasyData使用教程](https://www.bilibili.com/video/BV1A8411v7MW/?share_source=copy_web&vd_source=07343a22e791d6df8e8857b42ee60032)

#### 一个手势识别的项目
项目链接：[YOLO8实现实时手势识别附带源码和数据集](https://blog.csdn.net/Kdhbxndjdj/article/details/143901651)

#### 使用GPU加速训练
教程链接：[YOLOv8超详细环境搭建以及模型训练（GPU版本）](https://blog.csdn.net/2401_85556416/article/details/141394730)

---

