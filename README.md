# RaspSDR 固件镜像发布

RaspSDR（125MHz ADC 版 KiwiSDR）傻瓜式系统镜像：刷完即用，无需编译、无需配置。

## 当前版本

**v1.583-wf3-asym（125MHz，2026-09-14）** → [Releases 页面下载](../../releases)

已内置：

- KiwiSDR v1.583-wf3-asym 定制固件：4 音频接收通道 + 3 瀑布通道，通道 1 使用 16384 点 FFT / 16K 采样窗（其余为 8K）
- 125MHz ADC 时钟校正（频率精确，无音调偏移）
- 瀑布 27fps、CMA 抗颗粒显示、空闲 CPU 优化
- 62MHz 最大显示频率（max_freq=3）
- 首次开机自动扩容：8GB 以上任意大小的卡都能用满

## 怎么刷卡

从 Releases 下载 `RaspSDR-wf3asym-125MHz-20260914.img.xz`，任选一种工具：

1. **balenaEtcher**（推荐，最简单）：从 https://etcher.balena.io 下载安装，直接选择 `.img.xz` 文件 → 选 SD 卡（8GB 及以上）→ Flash，无需解压
2. **Raspberry Pi Imager**：选择"使用自定义镜像"→ 选 `.img.xz` 文件 → 选卡 → 写入
3. **Win32DiskImager**：先用 7-Zip 把 `.img.xz` 解压成 `.img`，再写入

## 首次开机

1. 插好网线（必须用网线），SD 卡插上，通电
2. 等 1~2 分钟（第一次开机会自动扩容分区，别断电）
3. 在路由器管理页面里找主机名为 `raspsdr` 的设备，记下它的 IP 地址

## 开始使用

- 浏览器打开 `http://<设备IP>:8173`
- 默认无密码，直接进入收听界面
- 建议马上做两件事：
  1. 点左上角 KiwiSDR 图标 → Admin → Security 标签，设置管理员密码
  2. Admin → Settings 里填写呼号、位置（位置也可自动获取）

## 管理员 / SSH

- 网页管理员密码：默认为空，首次进入 Admin 页自行设置
- SSH：用户 `root`，初始密码 `raspsdr`（仅限本地网络排障用，请尽快登录后执行 `passwd` 改成自己的密码）
- 刷卡后每一台的 SSH 主机密钥相同，介意的话执行：
  `rm /etc/ssh/ssh_host_* && dpkg-reconfigure openssh-server && reboot`

## 技术信息

- 固件选择：`SDR_RX4_WF3_ASYM`（firmware_sel=4）
- FPGA 位流：`RaspSDR.rx4.wf3.bit`（内置于 `/usr/local/bin/`）
- Web 端口：8173
- GPS：默认关闭（省 CPU，需要时去掉 `/etc/init.d/kiwid` 里的 `-gps` 参数）
- 源码：https://github.com/bh4rrg/Beagle_SDR_GPS 分支 `wf3-asym-125mhz`

## 已知注意事项

- 本镜像仅适用于 RaspSDR（125MHz ADC）硬件，不要刷到其他型号
- 卡容量 ≥8GB 均可，首次开机自动扩容到整卡
- 管理页 daily_restart（每日自动重启）默认开启，可在 Admin 页关闭
- 若网页打不开：确认网线、路由器 DHCP，然后给设备断电重启一次
