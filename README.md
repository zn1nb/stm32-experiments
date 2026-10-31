# STM32 基础实验项目

机器人工程竞赛入门自学仓库。目标：一个月内完成 10 个 STM32 基础外设实验，
作为竞赛小组入组考核材料（计划两周完成主体，余下时间做综合验证与复盘）。

## 项目说明

本仓库用于存放实验源码、接线图、自学笔记与踩坑记录。

每个实验一个独立目录，目录内包含：

- 完整 Keil 工程（`Project.uvprojx`）
- 源码（`User/` `Hardware/` `System/` `Library/` `Start/`）
- `接线图.jpg` —— 对应实验的接线图（压缩版，约 200 KB）

编译产物（`Objects/` `Listings/` `*.uvguix.*` `*.uvoptx`）已通过 `.gitignore` 排除，
clone 下来用 Keil 重新编译即可。

## 实验进度

| # | 实验 | 江协章节 | 目录 | 验收现象 | 状态 |
|:-:|------|:--------:|------|----------|:----:|
| 1 | LED 闪烁 | 3-1 | `exp01-led` | 板载 LED 1 Hz 闪烁 | ✅ |
| 2 | LED 流水灯 | 3-2 | `exp02-led` | 8 个 LED 依次点亮 | ✅ |
| 3 | 蜂鸣器 | 3-3 | `exp03-fengming` | 上电鸣叫 | ✅ |
| 4 | 按键控制 LED | 3-4 | `exp04-kongzhi` | 按下翻转，长按不连发 | ✅ |
| 5 | OLED 显示屏 | 4-1 | `exp05-oled` | 显示字符串与实时变量 | ✅ |
| 6 | 串口通信 | 9-2 | `exp06-chuankou` | 串口助手收发，OLED 显示十六进制 | ✅ |
| 7 | 旋转编码器计次 | 5-2 | `exp07-xuanzhuan` | 正转加、反转减，OLED 显示 | ✅ |
| 8 | 定时器定时中断 | 6-1 | `exp08-dingshizhongduan` | Num 每秒 +1，CNT 实时滚动 | ✅ |
| 9 | 定时器外部时钟 | 6-2 | `exp09-dingshiqiwaibu` | 外部脉冲触发计数 | ✅ |
| 10 | 外部中断（红外对射） | 5-1 | — | 遮挡一次，计数 +1 | ⬜ |
| 11 | PWM 输出 | 6-3 / 6-4 | — | 呼吸灯 + 舵机 0/90/180° | ⬜ |
| 12 | ADC 模拟采集 | 7-1 | — | 拧电位器，电压 0→3.3V | ⬜ |
| 13 | I2C 读取 MPU6050 | 10-1 | — | 串口打印三轴加速度 | ⬜ |
| 14 | 小型综合实验 | — | — | 按键切换模式，OLED 显示 | ⬜ |

图例：⬜ 未开始 · 🟨 进行中 · ✅ 完成

> 路径说明：实验编号按本仓库自身顺序排列，与江协科技教程的章节号不同；
> 对照关系见上表「江协章节」一列。

## 开发环境

- 主控芯片：STM32F103C8T6（最小系统板）
- 开发工具：Keil MDK 5.24 + ARMCC 5.06
- 固件库：STM32 标准外设库 V3.5.0
- 下载器：ST-Link V2
- 参考教程：江协科技《STM32 入门教程》

## 目录结构

```text
stm32-experiments/
├── README.md
├── exp01-led/                      # 3-1  LED 闪烁
│   ├── Hardware/  Library/  Start/  System/  User/
│   ├── Project.uvprojx
│   └── 接线图.jpg
├── exp02-led/                      # 3-2  LED 流水灯
├── exp03-fengming/                 # 3-3  蜂鸣器
├── exp04-kongzhi/                  # 3-4  按键控制 LED
├── exp05-oled/                     # 4-1  OLED 显示屏
├── exp06-chuankou/                 # 9-2  串口收发
├── exp07-xuanzhuan/                # 5-2  旋转编码器计次
├── exp08-dingshizhongduan/         # 6-1  定时器定时中断
└── exp09-dingshiqiwaibu/           # 6-2  定时器外部时钟
```

## 踩坑记录

- **定时器不准**：先在 `main` 里显示 `SystemCoreClock`，确认跑的是 72 MHz 而不是内部 8 MHz；
  晶振未起振时定时器会慢约 9 倍。
- **串口收不到**：TX/RX 必须交叉接，GND 必须共地，波特率与代码一致（本仓库为 9600）。
- **外部中断不触发**：忘记开 `AFIO` 时钟时 `GPIO_EXTILineConfig` 是空操作。
- **旋转编码器计数偏少**：编码器公共端（C）必须接 GND，否则只有一半边沿能触发。
