# omen-fan-b1xxx
- 手动控制 HP Omen 笔记本电脑风扇的简单实用程序
- 经测试发现，我的OMEN8 因缺少hwmon导致无法获取风扇转速以及最大转速文件，导致程序运行失败，故加入了一个补丁
- 对于源程序的服务，仓库中加入了服务启停文件以及systemctl的service文件，用于开机自启，使用时请自行将其放入对应文件夹下

```bash
# 首先你需要修改运行服务中的运行路径，为你环境中的路径 omen-fan.service:
ExecStart=/home/YOU_USER_NAME/.../omen-fan/omen-fan.sh
ExecReload=/home/YOU_USER_NAME/.../omen-fan/omen-fan.sh
ExecStop=/home/YOU_USER_NAME/.../omen-fan/omen-fan-stop.sh

# 加入到系统级服务
mv ./omen-fan.service /etc/systemd/system/omen-fan.service

# 重新加载服务
sudo systemctl daemon-reload

# 开启服务自启
sudo systemctl enable --now omen-fan.service

# 查看服务状态以及详细日志
systemctl status omen-fan-control.service
journalctl -u omen-fan-control.service -b --no-pager
```

- 查看电脑版本，如果没有找到相关hwmon文件的话不影响相应的转速控制功能，但无法查看风扇转速，可使用sensors命令来查看温度，以及通过感受风扇响声来验证转速是否发生变化

```bash
# 可使用如下命令查看电脑型号
> cat /sys/class/dmi/id/product_name
OMEN by HP Laptop 16-b1xxx
```

- 根据温度曲线进行转速调节

```bash
# 运行服务后会自动生成如下配置文件：/etc/omen-fan/config.toml
# Created by omen-fan script

[service]
TEMP_CURVE = [30, 40, 50, 60, 70, 80, 87, 93]
SPEED_CURVE = [50, 60, 70, 80, 88, 95, 95, 100]
IDLE_SPEED = 0
POLL_INTERVAL = 1

[script]
BYPASS_DEVICE_CHECK = 0

# 此时可修改TEMP_CURVE与SPEED_CURVE 来自定义温度转速曲线
```



# WARNING

使用时请注意设备以及人身安全，避免过猛的参数配置
