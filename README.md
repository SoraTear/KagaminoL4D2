# 鏡野屋的求生之路2服务器
>[玩家点我](#玩家须知)

本仓库收录服务器的开设、插件及维护信息
## 开服
> [参考文章:KOFXIII 《求生之路2服务器搭建》](https://www.bilibili.com/opus/736922474423255104)

2核2G服务器，Ubuntu 24.04 64bit  
安全组：入方向放行TCP 22、UDP 27015  
推荐运行在非root用户上，这里安装依赖库及配置用户`server`：
```Shell
sudo apt update
sudo apt install lib32gcc-s1
adduser server
```
在root用户上编辑`/etc/sudoers`文件，在root后加一行类似的：
```d
root    ALL=(ALL:ALL) ALL
server  ALL=(ALL:ALL) ALL 
```
切换到`server`用户，下载SteamCMD
```Shell
su server
mkdir Steam
cd Steam
sudo wget https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz
tar xvf steamcmd_linux.tar.gz && rm steamcmd_linux.tar.gz
./steamcmd.sh
```
在SteamCMD的shell中执行：
```Shell
force_install_dir /home/server/Steam/l4d2
login YOUR_ACCOUNT
app_update 222860 validate
```
至此游戏文件下载完成，安装[SourceMOD](https://www.sourcemod.net/downloads.php?branch=stable)和[MetaMOD](http://metamodsource.net/downloads.php?branch=stable)到`/home/server/Steam/l4d2/left4dead2`中（合并`addons`和`cfg`文件夹，以后的`addons`和`cfg`均指这两个文件夹）  
给玩家添加管理员权限：修改`addons/sourcemod/configs/admins_simple.ini`，在文件末尾添加：
```ini
"STEAM_x:x:xxxxxx"  "99:z"
```
在`cfg`下新建`server.cfg`，配置服务器基础设置  
最后在`/home/server`下配置启动脚本`l4d2StartServer.sh`：
```Shell
/home/server/Steam/l4d2/srcds_run -game left4dead2 -insecure +hostport 27015 -condebug -tickrate 100 +map c1m2_streets +exec server.cfg
```
然后
```Shell
chmod +x l4d2StartServer.sh
```
至此无插件的游戏服务器配置完成
## 插件
包括服务器的所有插件，均安装在`addons`和`cfg`下  
此外，三方图以vpk形式也安装到`addons`下
### 功能类
[Tickrate Enabler](https://github.com/accelerator74/Tickrate-Enabler/releases/tag/build)：服务器设置100tick  
以下插件来自[豆瓣酱插件整合包](http://l4d.vihh.net/forum.php?mod=viewthread&tid=75422)：  
Left 4 DHooks：允许插件调用游戏引擎内部功能  
WeaponHandling：允许插件修改武器行为  
原生键值函数库：允许插件调用游戏自带的键值函数  
原生投票函数库：允许插件进行自定义投票  
SourceScramble：允许插件补丁游戏引擎  
点燃或打爆物品提示：顾名思义  
扔投掷物提示：顾名思义  
伤害显示：打中特感时在特感身上显示伤害数字  
生还者对女巫的伤害排名：顾名思义  
生还者对坦克的伤害排名：顾名思义  
幸存者死亡和被制服提示：顾名思义  
排行榜插件：一个功能很多的HUD
### 三方图列表
此条目记录当前服务器所用的三方图及其对应的vpk号  
|地图名称|版本|包数|vpk号|steam链接|
|:---:|:---:|:---:|:---:|:---:|
|广州增城|8.06|1|2396847377|[Part1](https://steamcommunity.com/sharedfiles/filedetails/?id=2396847377)|
|广西灵山|3.1|3|3583374422<br>3583375624<br>3583381403|[Part1](https://steamcommunity.com/sharedfiles/filedetails/?id=3583374422)<br>[Part2](https://steamcommunity.com/sharedfiles/filedetails/?id=3583375624)<br>[Part3](https://steamcommunity.com/sharedfiles/filedetails/?id=3583381403)|
### 修复类
以下插件来自[豆瓣酱插件整合包](http://l4d.vihh.net/forum.php?mod=viewthread&tid=75422)：  
修复地图过渡数据遗产：清理换图残留数据  
修复电击器救起存活的幸存者：修复电击器  
解决 Linux 系统 CFG 不加载：配置命令缓冲区修复
## 玩家须知
在自己的 Steam 求生之路2安装目录（库右键 Left 4 Dead 2 → 属性 → 已安装文件 → 浏览）中找到left4dead2文件夹 → cfg文件夹  
在cfg文件夹下新建一个文件名为`autoexec.cfg`，复制以下内容并保存：
```javascript
rate 30000
cl_cmdrate 100
cl_updaterate 100
cl_interp_ratio 0
cl_interp 0
```
如果想玩三方图，需要参照[这里](#三方图列表)在创意工坊订阅对应的三方图（如果有必需订阅在订阅时选“全部订阅”），进入游戏等待“附加内容”转好并确认启用了三方图方可进入服务器游玩  
对插件有建议?提出Pull Request!
## 维护
启动服务器：`./l4d2StartServer.sh`  
关闭服务器：`quit`  
查询状态：`status`
### screen使用
监听命令`Ctrl + A`；命令：托盘会话`D`、退出会话`K`  
新建会话`screen -S SESSION_NAME`  
会话列表`screen -ls`  
恢复对话`screen -r SESSION_NAME`  
[TODO](./TODO.md)