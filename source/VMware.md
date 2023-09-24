#### 下载

官方下载: https://customerconnect.vmware.com/en/downloads/info/slug/desktop_end_user_computing/vmware_workstation_pro/17_0 (需要注册账号登录，比较麻烦，可看下一节，也可直接国内下载)

国内下载: https://pc.qq.com/detail/0/detail_21600.html

---

#### 账号注册

注册网址: https://customerconnect.vmware.com/account-registration

1. 除了Address2外均要填。最下方的I agree to the...即同意协议需要勾选，其余不用。

2. 密码要求: 8-20位，小写字母，大写字母，数字，特殊符号每种至少一个。

3. 邮编验证比较麻烦，可填下列内容:

   Country: China

   Province: Beijing

   City: BeiJing

   postal code: 100103

---

#### 安装和激活

1. 安装过程自行决定。(增强型键盘驱动程序: 更好地处理国际键盘和带有额外按键的键盘)
2. 激活密钥: `ZF3R0-FHED2-M80TY-8QYGC-NPKYF` **<u>实测版本16.2.4可用</u>**

**注1**: 若安装过程中出现 **安装程序检测到主机启用了hyper-v** 提示，可进行如下方法关闭。(关闭后也可提高电脑性能)

**注2**: 本机为Win11家庭版，其余版本可通过控制面板 --> 程序和功能 --> 启用或关闭Windows功能 --> 去掉Hyper-V关闭。(未测试)

> 方法1

1. 打开Windows安全中心。
2. 找到 设备安全性 --> 内核隔离 --> 内核隔离详细信息 --> 关闭内存完整性
3. 重启设备生效。

>方法2

1. win+R打开运行窗口执行 `msinfo32` 打开系统信息。
2. 查看系统摘要中的 基于虚拟化的安全性 此时其处于正在运行状态。
3. 下载工具并解压 https://www.microsoft.com/en-us/download/details.aspx?id=53337
4. 以管理员方式打开 Windows PowerShell 执行 `set-ExecutionPolicy RemoteSigned` 并输入Y
5. 执行 `Set-location 解压位置`。 如 `Set-location D:\InstallationPackage\dgreadiness_v3.6`。
6. 执行 `.\DG_Readiness_Tool_v3.6.ps1 -Disable`，报错不管。
7. 重启系统，出现内容后连按两次F3即可。开机后可查看系统信息是否更改。

---

#### 镜像下载

镜像站: https://developer.aliyun.com/mirror/

1. 选择需要下载的镜像类型。
2. 进入简介中的下载地址。
3. 依次选择 版本号 --> isos --> x84_64。(本机选的是7.9)
4. 可以下载DVD版本(完整版)，也可以下载Minimal版。

---

#### 安装系统(以centos为例)

> 新建向导

流程: 典型 --> 稍后安装操作系统 --> Linux CentOS 7 64位 --> 修改存放位置 --> 默认 --> 自定义硬件。

**注**: 最后一步可以将USB/声卡/打印机删除，内存和处理器根据本机性能调整，DVD使用ISO映像文件即下载的镜像。

> 安装向导

1. 开启虚拟机，点击内部屏幕进入虚拟机操作，Ctrl+G可返回本机操作。
2. 默认选择Test this media...即先检查设备再安装，可以按 ↑ 选择第一个选项直接开始安装。
3. 图形化界面出现后选择语言进行下一步安装配置。
4. 时间和日期 可以使用网络时间，需要先配置网络才能启用，配置网络参考7，推荐联网获取时间后就关闭网络。
5. 软件安装 可选择预装软件，最小安装=命令行界面，GNORE桌面=图形化界面。**此处本机安装命令行界面**
6. 安装位置 选择新建向导中分配给虚拟机的磁盘即可，也可参考后续介绍的分区方案。
7. 网络和主机名 可连接网络和修改主机名(需点击应用)，以太网旁按钮点击后可连接网络。(此时会默认分一个IP地址，后续可改)
8. 开始安装 需设置root用户密码

> 分区方案

分区原因: https://so66.cn/82295.html (方便管理/增加安全性/提高性能)

操作步骤: 安装位置 --> 我要配置分区 --> 完成 --> 分区方案选为标准分区 --> 新增以下分区(数值自选，可百度看推荐)

- /boot: 1024M
- swap: 1024M
- biosboot: 1M
- /: 不输入默认将剩余空间分配

点击完成，接受更改即可。

> 网络配置

1. 选择VMware导航栏中编辑里的虚拟网络编辑器，记录VMnet8对于的子网地址。(以192.168.19.0为例)

**注**: 也可以点击更改设置修改安装时给定的地址，但不推荐。

2. 开启虚拟机，以root用户登录，编辑网络配置文件。

```bash
vi /etc/sysconfig/network-scripts/ifcfg-ens32
```

- BOOTPROTO: 改为"static"。表示使用静态IP。
- ONBOOT: 改为"yes"。表示开机自启。
- 新增 **"IPADDR": "192.168.19.3"**。为本台虚拟机设置IP地址，最后一位范围3-254。(1代表本机，2被网关占用)
- 新增 **"GATEWAY": "192.168.19.2"**。为本台虚拟机设置网关。
- 新增 **"DNS1": "8.8.8.8"**。为本台虚拟机设置DNS解析服务器1。(可加DNS2...，即便只有1个也必须写DNS<u>1</u>)

3. 重启网络服务并测试。

```bash
service network restart
ping www.baidu.com
```

---

#### 外接XShell终端

下载地址: https://www.xshell.com/zh/free-for-home-school/ (学生版，免费，至多开五个窗口)

安装注册: 安装过程自选。打开软件后填用户名和邮箱，会发邮件到邮箱验证。

连接虚拟机: 文件 --> 新建 --> 名称自定义 --> 主机填虚拟机设置的IP地址 --> 连接并保存密钥 --> 输入用户名密码登录即可。
