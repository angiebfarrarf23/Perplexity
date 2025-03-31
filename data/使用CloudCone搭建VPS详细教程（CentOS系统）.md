# 使用CloudCone搭建VPS详细教程（CentOS系统）

## 基本操作命令

- 包括 `--pid-file ss1.pid` 等常用参数

## 第一步：选择服务器配置

以CloudCone促销信息为准（最低$2/月）

👉 [【点击查看】2025年最新CloudCone优惠码及特价云服务器方案汇总](https://bit.ly/Cloudcone)

**创建注意事项：**
1. 在Networking选项中开启IPv6支持
2. 初始密码会通过邮件发送，也可在Access面板重新获取

## 第二步：连接服务器

使用Xshell等SSH工具连接：
- 服务器IP
- 用户名：root
- 初始密码（邮件获取）

**修改root密码：**
bash
passwd
# 按提示输入新密码两次

## 第三步：安装Shadowsocks服务

bash
# 安装并更新pip
curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
python get-pip.py
pip install --upgrade pip

# 安装Shadowsocks
pip install shadowsocks

# 创建配置文件
vi /etc/shadowsocks.json

配置文件示例：
json
{
  "server":"::",
  "port_password":{
    "8388":"your_password1",
    "8366":"your_password2"
  },
  "timeout":300,
  "method":"aes-256-cfb"
}

## 第四步：启动服务

**手动启动方式：**
bash
ssserver -c /etc/shadowsocks.json -d start

**系统服务方式（推荐）：**
bash
vim /etc/systemd/system/shadowsocks.service

服务文件内容：
ini
[Unit]
Description=Shadowsocks

[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/ssserver -c /etc/shadowsocks.json

[Install]
WantedBy=multi-user.target

**管理服务：**
bash
# 设置开机启动
systemctl enable shadowsocks

# 启动服务
systemctl start shadowsocks

# 查看状态
systemctl status shadowsocks -l

## 第五步：防火墙设置

bash
# 开放端口
firewall-cmd --zone=public --add-port=8388/tcp --permanent
firewall-cmd --zone=public --add-port=8366/tcp --permanent
firewall-cmd --reload

## 系统优化建议

选择CloudCone的 **CentOS 7.5 Server - 64 Bit (Latest Kernel + BBR)** 镜像，默认已启用BBR加速算法，可显著提升网络性能。