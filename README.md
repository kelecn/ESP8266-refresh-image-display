# 基于ESP8266的局域网图片刷新显示系统
简介：简单的说就是通过网页将图片上传至NodeMCU（ESP8266）的flash闪存，再将图片数据通过SPI更新至TFT-LCD显示屏进行显示。

![](https://7.dusays.com/2021/04/30/623386b314664.png)
### 一、硬件系统

<hr>


硬件主要用到NodeMCU（ESP8266）和1.44寸TFT-LCD彩色液晶屏，[淘宝](https://item.taobao.com/item.htm?spm=a230r.1.14.1.4909234fdyizrn&id=531755241333&ns=1&abbucket=0#detail)都有得卖，也不贵。


|   硬件   |                型号                |      价格      |
| :------: | :--------------------------------: | :------------: |
| WiFi模块 |         NodeMCU（ESP8266）         | 十几到二十不等 |
|  显示屏  | 1.44寸TFT-LCD液晶屏（SKU:MAR1442） | 十几到二十不等 |
|   排线   |               母对母               |     一两块     |

- NodeMCU（ESP8266）IO口介绍：

![](https://7.dusays.com/2021/05/01/2ebb6c7ff7a03.png)

- 1.44寸TFT-LCD液晶屏IO口介绍：

| 标号 | PIN  | ESP8266开发板对应的接线引脚 |                 引脚说明                  |
| :--: | :--: | :-------------------------: | :---------------------------------------: |
|  1   | VCC  |             3V              |                  电源正                   |
|  2   | GND  |              G              |                  电源地                   |
|  3   | GND  |             \-              |                  电源地                   |
|  4   |  NC  |             \-              |         无定义，保留，不需要接线          |
|  5   |  NC  |             \-              |         无定义，保留，不需要接线          |
|  6   | LED  |             3V              | LCD背光控制信号（如不需要控制，请接3.3V） |
|  7   | CLK  |             D5              |            LCD SPI总线时钟引脚            |
|  8   | SDI  |             D7              |            LCD SPI总线数据引脚            |
|  9   |  RS  |             D1              |        LCD寄存器/数据选择控制引脚         |
|  10  | RST  |             D2              |              LCD复位控制引脚              |
|  11  |  CS  |             D8              |              LCD片选控制引脚              |

- NodeMCU（ESP8266）与1.44TFT-LCD显示屏接线图：

![](https://7.dusays.com/2021/05/01/0150e73282eec.png)



### 二、软件系统

<hr>




**1、开发环境搭建**


1. NodeMCU硬件通过USB连接电脑，需提前安装好CH340USB[串口驱动](http://www.wch.cn/downloads/CH341SER_EXE.html)。

2. 电脑端提前安装好[Arduino](https://www.arduino.cc/en/software)开发平台。

3. 安装ESP8266开发板：打开Arduino IDE点击菜单栏的【文件】-【首选项】，添加【附加开发板管理器】网站：https://arduino.esp8266.com/stable/package_esp8266com_index.json 。

   ![](https://7.dusays.com/2021/05/01/1ab6a2dc11779.png)

   然后点击【工具】->【开发板】->【开发板管理器】，搜索 `esp8266` 后安装。

   ![](https://7.dusays.com/2021/05/01/bfdbdb8c8e2b3.png)

4. 安装第三方显示屏支持库：将文件夹（Adafruit_ST7735_Library、Adafruit-GFX-Library）移动到Arduino安装目录下的libraries文件夹中，重启Arduino IDE，即可，也就是编程环境搭建完毕。

5. 可选择性安装mDNS服务，安装后，可在浏览器输入域名（host.local）实现访问ESP8266的Web页面，若不安装mDNS服务则通过访问ESP8266实际分配的IP地址实现Web访问。

   - Mac OS：默认自带mDNS
   - Windows：需安装[Bonjour](https://support.apple.com/kb/DL999?viewlocale=en_US&locale=en_US)
   - Linux：需安装[avahi](http://avahi.org/)

**2、软件程序**

软件实现通过网页将图片上传至NodeMCU（ESP8266），并将图片更新至TFT-LCD显示屏。主要的程序流程图如下：

<img src="https://7.dusays.com/2021/05/01/73e40087635a5.png" style="zoom: 25%;" width="auto" height="auto" align="middle" />

### 三、效果演示

<hr>




**1、配网**

配置WiFi或热点的名称和密码、设定mDNS地址。

<img src="https://7.dusays.com/2021/05/01/a9bc9ef1671d8.png" style="zoom: 33%;" />

**2、程序下载**

将下载程序到开发版，待ESP8266与电脑连接上同一个WiFi（或者是电脑开的热点），就可以从串口监视器看到IP地址。

<img src="https://7.dusays.com/2021/05/01/89ac6760a38fe.png" style="zoom:;" />

若是使用手机热点，也可以通过[终端模拟器](https://cdn.jsdelivr.net/gh/kelecn/ESP8266-refresh-image-display@master/IP%E6%9F%A5%E8%AF%A2%E8%BD%AF%E4%BB%B6/%E7%BB%88%E7%AB%AF%E6%A8%A1%E6%8B%9F%E5%99%A8_1.0.70.apk)应用终端输入`ip neigh`进行查看连接到手机热点的设备IP地址。

**3、图片上传**

访问该IP地址即可，访问图片上传Web页面，选择图片上传即可。

![](https://7.dusays.com/2021/05/01/deb2e1cbaed96.png)

**4、显示效果**

| <img src="https://7.dusays.com/2021/05/01/67b682e9ada8f.jpg" style="zoom: 25%;" /> | <img src="https://7.dusays.com/2021/05/01/79a3e8fbcbb4c.jpg" style="zoom:25%;" /> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://7.dusays.com/2021/05/01/035c4c302e6b3.jpg" style="zoom:25%;" /> | <img src="https://7.dusays.com/2021/05/01/6c451a1e4ae45.jpg" style="zoom:25%;" /> |

### 四、参考资料

<hr>


1、[ESP8266 TFT(ST7735)彩屏-web刷图](https://www.arduino.cn/thread-42247-1-1.html) 

2、[ESP8266网络应用5 - 设置mdns](https://www.bilibili.com/video/BV1Jc411h767)