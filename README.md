# 鏡野屋的求生之路2服务器
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
/home/server/Steam/l4d2/srcds_run -game left4dead2 -insecure +hostport 27015 -condebug +map c1m2_streets +exec server.cfg -nomaster
```
然后
```Shell
chmod +x l4d2StartServer.sh
```
至此无插件的游戏服务器配置完成