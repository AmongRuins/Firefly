---
title: Linux相关命令
published: 2026-04-04
description: '一些Linux常见命令'
image: './images/covers/linux.webp'
tags: [Linux]
category: '软件测试'
draft: false 
lang: ''
---

## 系统基础与信息查询

用于查看系统状态、内核信息、运行时间等基础信息。

| **命令**       | **核心作用**    | **高频用法与示例**                                                            |
|--------------|-------------|------------------------------------------------------------------------|
| **uname**    | 查看系统内核信息    | `uname -a` 查看全部系统信息；`uname -r` 查看内核版本                                      |
| **hostname** | 查看 / 设置主机名  | `hostname` 查看当前主机名；`hostnamectl set-hostname new-name` 永久修改主机名（systemd 系统） |
| **date**     | 查看 / 设置系统时间 | `date` 查看当前时间；`date -s ""2026-04-04 12:00:00""` 设置系统时间                     |
| **uptime**   | 查看系统运行时长与负载 | `uptime`显示开机时长、登录用户数、1/5/15 分钟系统平均负载                                    |
| **who/w**    | 查看当前登录用户    | `who` 简单显示登录用户；`w` 详细显示用户及正在执行的操作                                          |
| **dmesg**    | 查看系统内核开机日志  | `grep error` 排查开机硬件 / 系统错误                                                                 |
| **history**  | 查看命令执行历史    | `history` 查看全部历史；`!!` 执行上一条命令；`!n` 执行第 n 条历史命令；`history -c` 清空历史               |

## 文件与目录操作（最高频核心）

Linux 一切皆文件，该类命令是日常操作的基础。

1. 路径与目录导航
    ```bash
    pwd                # 显示当前工作目录的绝对路径
    cd /               # 切换到根目录
    cd ~               # 切换到当前用户家目录
    cd ..              # 切换到上一级目录
    cd -               # 切换到上一次所在的目录
    cd /usr/local      # 切换到指定绝对路径
    ```

2. 目录与文件创建 / 删除
    ```bash
    mkdir test         # 创建单级目录
    mkdir -p a/b/c     # 递归创建多级目录
    touch test.txt     # 创建空文件，或修改文件时间戳
    touch a.txt b.txt  # 批量创建空文件
    rmdir test         # 删除空目录（仅能删空目录）
    rm test.txt        # 删除文件
    rm -rf test_dir    # 强制递归删除目录及所有内容（高危！严禁执行 rm -rf /）
    ```

3. 目录内容查看
    ```bash
    ls                 # 列出当前目录内容
    ls -l              # 长格式显示（权限、所有者、大小、时间等，别名ll默认支持）
    ls -lha            # 显示所有文件（含隐藏文件）+ 人性化大小单位
    ls -lht            # 按修改时间倒序排列
    ```

4. 文件 / 目录复制与移动
    ```bash
    cp test.txt /tmp/          # 复制文件到指定目录
    cp -r test_dir /tmp/       # 递归复制目录（必须加-r）
    mv test.txt new.txt        # 重命名文件
    mv test.txt /tmp/          # 移动文件到指定目录
    ```

5. 文件查找
    ```bash
    # 语法：find [路径] [条件] [动作]
    find / -name "test.txt"    # 全局按文件名精确查找
    find . -name "*.log"       # 当前目录查找所有.log结尾的文件
    find . -type f -size +100M # 查找当前目录大于100M的文件
    find . -mtime -7            # 查找7天内修改过的文件
    find . -name "*.tmp" -delete # 查找并删除.tmp文件
    ```

6. 链接创建
    ```bash
    ln -s /target/path /link/path  # 创建软链接（符号链接，类似Windows快捷方式）
    ln /target/file /link/file      # 创建硬链接（仅限文件，不能跨分区）
    ```

## 文件内容查看与编辑

用于查看日志、修改配置、文本过滤等核心场景。

1. 文件内容查看
    ```bash
    cat test.txt         # 查看小文件全部内容，-n 显示行号
    more test.log        # 分页查看大文件，向下翻页，q退出
    less test.log        # 分页神器，支持上下翻页、/关键词搜索，q退出（大日志首选）
    head -20 test.txt    # 查看文件前20行（默认10行）
    tail -50 test.txt    # 查看文件最后50行（默认10行）
    tail -f test.log     # 实时追踪文件新增内容（看日志必备）
    ```

2. 文本过滤与统计
    ```bash
    # grep 正则搜索过滤，高频核心
    grep "error" test.log        # 查找包含error的行
    grep -i "error" test.log     # 忽略大小写匹配
    grep -v "info" test.log      # 反向匹配，排除含info的行
    grep -n "root" /etc/passwd   # 显示匹配行的行号
    grep -rn "keyword" /etc/     # 递归搜索目录下所有文件的关键词

    # wc 统计
    wc -l test.txt                # 统计文件行数
    wc -w test.txt                # 统计单词数
    ls | wc -l                    # 统计当前目录文件数量
    ```

3. vim/vi 文本编辑器

    Linux 系统标配编辑器，核心分为 3 种模式，高频操作如下：
    ```bash
    vim test.txt    # 打开/创建文件，默认进入【命令模式】
    ```

    - 命令模式 → 插入模式：按 `i(光标前插入)、a(光标后插入)、o(下一行新建)`
    - 插入模式 → 命令模式：按 `ESC`
    - 命令模式 → 底行模式：按 `:`

    底行模式高频操作：
    ```bash
    :w          # 保存文件
    :q          # 退出
    :wq         # 保存并退出
    :q!         # 强制退出不保存
    :%s/old/new/g  # 全局替换old为new
    ```

    命令模式高频操作：
    ```bash
    dd    # 删除当前行
    yy    # 复制当前行
    p     # 粘贴
    u     # 撤销上一步
    /关键词 # 向下搜索
    ?关键词 # 向上搜索
    ```

## 权限管理

Linux 多用户权限核心，文件权限分为`所有者 (u)`、`所属组 (g)`、`其他人 (o)`，权限对应：读 (r=4)、写 (w=2)、执行 (x=1)。

```bash
# chmod 修改文件/目录权限
chmod 755 test.txt        # 数字法：所有者rwx(7)，组/其他人rx(5)
chmod 644 test.txt        # 常用文件权限：所有者rw，其他只读
chmod -R 755 test_dir     # 递归修改目录下所有文件权限
chmod u+x test.sh          # 符号法：给所有者添加执行权限
chmod o-w test.txt         # 给其他人移除写权限

# chown 修改所有者和所属组
chown user:group test.txt  # 修改文件所有者为user，所属组为group
chown -R nginx:nginx /data # 递归修改目录所有者

# chgrp 修改所属组
chgrp group test.txt

# umask 查看/设置默认权限掩码
umask 022                  # 新建文件默认644，目录默认755
```

## 进程管理

用于排查服务状态、资源占用、进程启停。

```bash
# 进程查看
ps -ef                     # 查看全格式所有进程
ps aux                     # 查看所有进程，含CPU/内存占用
ps aux | grep nginx        # 过滤指定进程
top                        # 实时动态查看进程资源（任务管理器），P按CPU排序，M按内存排序，q退出
htop                       # top增强版，界面更友好（需额外安装）
pstree                     # 树状显示进程父子关系

# 进程终止
kill 1234                  # 正常终止PID为1234的进程（信号15）
kill -9 1234               # 强制杀死进程（信号9，慎用）
killall nginx              # 按进程名终止所有相关进程

# 进程后台运行与优先级
nohup ./test.sh &          # 后台运行脚本，关闭终端不终止，输出到nohup.out
jobs                       # 查看当前终端后台任务
fg %1                      # 把1号后台任务调到前台
bg %1                      # 把暂停的任务放到后台继续运行
nice -n 10 ./test.sh       # 启动时指定进程优先级（-20最高，19最低）
renice -5 -p 1234          # 修改已运行进程的优先级
```

## 网络管理

用于网络连通性排查、端口监听、接口测试、防火墙配置。

```bash
# 网卡与IP配置
ip addr                    # 查看所有网卡IP地址（替代ifconfig）
ip link set eth0 up/down   # 启用/禁用网卡
ip route                   # 查看系统路由表
ifconfig                   # 老版本网卡配置命令（需安装net-tools）

# 连通性与链路排查
ping baidu.com             # 测试网络连通性
ping -c 4 192.168.1.1     # 指定ping4次
traceroute baidu.com       # 路由追踪，排查网络链路
mtr baidu.com              # 增强版路由追踪（结合ping+traceroute）
telnet 192.168.1.1 80     # 测试TCP端口连通性

# 端口与网络连接查看
ss -tulnp                  # 查看所有监听的TCP/UDP端口，显示PID/进程名（netstat替代）
ss -tulnp | grep 80        # 过滤指定端口
netstat -tulnp             # 传统端口查看命令（需net-tools）

# 域名解析
host baidu.com             # 域名解析查询
nslookup baidu.com         # 域名解析详细信息

# 网页/接口请求与文件下载
curl https://baidu.com     # 命令行访问URL，测试接口
curl -I https://baidu.com  # 查看响应头
curl -O https://example.com/file.zip # 下载文件
curl -X POST -d "name=test" https://api.example.com # 发送POST请求
wget https://example.com/file.zip # 命令行下载文件
wget -c https://example.com/file.zip # 断点续传下载

# 防火墙管理
## firewalld（CentOS/RHEL/Rocky 7+）
firewall-cmd --state               # 查看防火墙状态
firewall-cmd --list-ports          # 查看已开放端口
firewall-cmd --add-port=80/tcp --permanent # 永久开放80端口
firewall-cmd --reload              # 重载防火墙规则使配置生效
firewall-cmd --remove-port=80/tcp --permanent # 关闭端口

## ufw（Ubuntu/Debian）
ufw status                  # 查看防火墙状态
ufw allow 80/tcp            # 开放80端口
ufw deny 80/tcp             # 关闭80端口
ufw enable/disable           # 启用/禁用防火墙
```

## 压缩与解压

Linux 主流压缩格式全支持，跨 Windows 兼容。

```bash
# tar 打包压缩（最常用，支持gzip/xz/bzip2）
## 核心选项：-c创建 -x解压 -z(gzip) -J(xz) -j(bzip2) -v显示过程 -f指定文件 -C指定解压目录
tar -zcvf test.tar.gz /test    # 打包并gzip压缩test目录
tar -zxvf test.tar.gz           # 解压.tar.gz/.tgz包
tar -zxvf test.tar.gz -C /tmp   # 解压到指定/tmp目录
tar -Jcvf test.tar.xz /test     # 打包并xz压缩（压缩率最高，大文件首选）
tar -Jxvf test.tar.xz            # 解压.tar.xz包

# zip/unzip 跨Windows通用格式
zip -r test.zip test_dir         # 递归压缩目录为zip包
unzip test.zip                    # 解压zip包
unzip test.zip -d /tmp            # 解压到指定目录
unzip -l test.zip                 # 查看zip包内容

# 单文件压缩
gzip test.txt                     # 压缩为test.txt.gz
gunzip test.txt.gz                # 解压gzip文件
xz test.txt                       # 压缩为test.txt.xz
unxz test.txt.xz                  # 解压xz文件
```

## 磁盘与存储管理

用于磁盘分区、挂载、空间占用排查。

```bash
# 磁盘空间查看
df -h                  # 查看磁盘分区使用情况，人性化显示大小
df -hT                 # 额外显示文件系统类型
du -sh test_dir        # 查看目录总占用大小
du -h --max-depth=1 /usr # 查看/usr下一级目录占用大小

# 磁盘分区与格式化
fdisk -l               # 查看所有磁盘分区信息（MBR分区）
parted -l              # 查看分区信息（支持GPT大磁盘）
mkfs.ext4 /dev/sda1    # 格式化分区为ext4文件系统
mkfs.xfs /dev/sda2     # 格式化分区为xfs文件系统

# 磁盘挂载与卸载
mount /dev/sda1 /data  # 把sda1分区挂载到/data目录
mount -o remount,rw /  # 重新挂载根目录为读写模式
umount /dev/sda1        # 按分区卸载
umount /data            # 按挂载点卸载
blkid                   # 查看分区UUID（用于/etc/fstab永久挂载）

# 内存与swap查看
free -h                 # 查看内存与swap分区使用情况
```

## 用户与用户组管理

多用户系统账号管理核心命令。

```bash
# 用户管理
useradd -m -s /bin/bash testuser # 创建用户，自动创建家目录，指定默认shell为bash
passwd testuser                   # 设置/修改用户密码
passwd                            # 修改当前用户密码
userdel -r testuser               # 删除用户，同时删除家目录与邮箱
usermod -aG sudo testuser         # 把用户添加到sudo组（赋予管理员权限）
usermod -s /bin/bash testuser     # 修改用户默认shell
id testuser                       # 查看用户UID、GID、所属组
id                                # 查看当前用户信息

# 用户组管理
groupadd testgroup                # 创建用户组
groupdel testgroup                # 删除用户组
gpasswd -a testuser testgroup     # 把用户加入指定组
gpasswd -d testuser testgroup     # 把用户从组中移除

# 用户切换
su testuser                       # 切换到指定用户
su - root                         # 切换到root用户，同时加载root环境变量
sudo command                      # 以root权限执行单条命令
```

## 软件包管理

不同发行版分为 RPM 系和 DEB 系，命令不通用，对应如下。

**RPM 系（CentOS、RHEL、Rocky、AlmaLinux、Fedora）**
```bash
# yum（CentOS7及之前）/ dnf（CentOS8+/RHEL8+，兼容yum语法）
yum install -y nginx      # 安装软件，-y自动确认
yum remove nginx           # 卸载软件
yum update nginx           # 更新软件
yum update                 # 全系统更新
yum search nginx           # 搜索软件包
yum list installed         # 查看所有已安装软件
yum clean all              # 清理软件缓存

# rpm 底层包管理（直接操作.rpm包）
rpm -ivh xxx.rpm           # 安装rpm包
rpm -e nginx                # 卸载软件
rpm -qa                     # 查看所有已安装rpm包
rpm -ql nginx               # 查看软件安装的文件路径
rpm -qf /usr/bin/nginx      # 查看文件属于哪个rpm包
```

**DEB 系（Ubuntu、Debian、Linux Mint）**
```bash
# apt 高级包管理器（首选）
apt update                 # 更新软件源缓存
apt install -y nginx       # 安装软件
apt remove nginx           # 卸载软件（保留配置）
apt purge nginx            # 彻底卸载（删除配置）
apt upgrade                # 全系统更新已安装软件
apt search nginx           # 搜索软件
apt list --installed       # 查看已安装软件
apt clean                  # 清理缓存

# dpkg 底层包管理（直接操作.deb包）
dpkg -i xxx.deb            # 安装deb包
dpkg -r nginx              # 卸载软件
dpkg -P nginx              # 彻底卸载
dpkg -l                    # 查看所有已安装软件
dpkg -L nginx              # 查看软件安装路径
dpkg -S /usr/bin/nginx     # 查看文件所属包
```

## 系统服务管理（systemd）

主流 Linux 发行版（CentOS7+、Ubuntu16.04+）均采用 systemd 管理系统服务，核心命令 `systemctl`。

```bash
systemctl start nginx        # 启动服务
systemctl stop nginx         # 停止服务
systemctl restart nginx      # 重启服务
systemctl reload nginx       # 平滑重载配置（不重启进程）
systemctl status nginx       # 查看服务运行状态、日志
systemctl enable nginx       # 设置服务开机自启
systemctl disable nginx      # 关闭开机自启
systemctl enable --now nginx # 开启自启并立即启动服务
systemctl list-unit-files --type=service # 查看所有服务开机自启状态

# 系统电源管理
systemctl reboot             # 重启系统
systemctl poweroff           # 关机
```

## 高级文本处理神器

**awk 按列处理文本**
```bash
awk '{print $1}' test.txt                # 打印文件第一列
df -h | awk '{print $1,$5}'              # 打印磁盘设备名与使用率
awk -F: '{print $1,$3}' /etc/passwd      # 以:为分隔符，打印用户名与UID
awk '$3>1000' /etc/passwd                # 过滤UID大于1000的行
```

**sed 流编辑器（文本替换、增删）**
```bash
sed 's/old/new/g' test.txt               # 全局替换old为new（不修改原文件）
sed -i 's/old/new/g' test.txt            # 直接修改原文件内容
sed '2d' test.txt                         # 删除第二行
sed '2i hello' test.txt                   # 在第二行前插入hello
sed '2a world' test.txt                   # 在第二行后追加world
```

## 高频实用符号与快捷键

核心符号：
- **`**：管道符，前一个命令的输出作为后一个命令的输入
- `>`：覆盖重定向：清空文件原有内容写入
- `>>`：追加重定向：在文件末尾追加内容
- `&`：后台运行符：命令在后台执行
- `xargs`：标准输入转命令行参数

效率快捷键：
- `Ctrl + C`：终止当前正在运行的命令
- `Ctrl + L`：清屏，等效于 clear
- `Ctrl + R`：搜索历史命令
- `Ctrl + A`：光标跳到行首
- `Ctrl + E`：光标跳到行尾
- `Tab`：命令 / 路径自动补全，按两次显示所有匹配项

## 注意事项

1. 执行高危命令（`rm -rf、mkfs、fdisk`等）前，务必确认路径与参数，避免误操作导致数据丢失。
2. 普通用户权限不足时，需加 `sudo` 或以 `root` 用户执行。
3. 所有命令的完整官方用法，均可通过 `man 命令名` 查看帮助手册。