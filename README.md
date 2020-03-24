## 基于 docker 的 confluence 安装


## Requirments

* 版本随系统即可
- centos 7.+
- docker
- docker compose
- git


#### Deployment

```
1. clone project

$ git clone https://github.com/Springzhen/confluence.git

2. start docker-compose

$ sudo docker-compose up -d

3. install step by step

- 页面启动

http://ip:3090
选择企业版本安装，下一步进入激活页面，记录ServiceID


- 破解confluence

step 1. 复制并备份文件到本地crackfile/iNViSiBLE文件夹中
$ cd ./confluence/crackfile
$ docker cp confluence:/opt/atlassian/confluence/confluence/WEB-INF/lib/atlassian-extras-decoder-v2-3.2.jar ./iNViSiBLE/atlassian-extras-2.4.jar

step 2. 使用破解工具生成新的jar文件
将iNViSiBLE文件夹下载到windows 电脑中，双击iNViSiBLE下的keygen.bat，打开破解界面，将开始记录的ServiceID 输入，name可以随意填写
点击patch选择刚改名的文件（iNViSiBLE文件夹中的atlassian-extras-2.4.jar）点击.gen！生成key，则atlassian-extras-2.4.jar已经破解完成
将atlassian-extras-2.4.jar改回atlassian-extras-decoder-v2-3.2.jar并上传到服务器iNViSiBLE文件夹中，同时记录破解界面中的激活码
将刚上传到iNViSiBLE文件夹中的atlassian-extras-decoder-v2-3.2.jar复制回confluence容器中原先目录：
$ docker cp ./iNViSiBLE/atlassian-extras-decoder-v2-3.2.jar confluence:/opt/atlassian/confluence/confluence/WEB-INF/lib/

step 3. 重启confluence容器
docker restart confluence
打开web界面 http://ip:3090，在serviceid 激活界面粘贴step 2中的激活码，点击下一步即可激活完成

step 4. 配置外置数据库postgresql，通过jdbc配置连接即可
jdbc:postgresql://ip:5434/wiki
user:wiki
passwd:wikijoker

配置完成后开启防火墙访问端口：5434 3090 3091
$ firewall-cmd --add-port=5434/tcp --permanent
$ firewall-cmd --add-port=3090/tcp --permanent
$ firewall-cmd --add-port=3091/tcp --permanent
重新载入添加的端口
$ firewall-cmd --reload
查询指定端口是否开启成功
firewall-cmd --query-port=5434/tcp
firewall-cmd --query-port=3090/tcp
firewall-cmd --query-port=3091/tcp

开放防火墙端口后继续执行下一步即可完成安装。

step 5. 添加管理用户
具体操作参考[wiki官方手册](https://www.cwiki.us/collector/pages.action?key=CONFLUENCEWIKI)

```


## Reference

- [基于docker 的confluence 安装 和破解](https://blog.csdn.net/MatrixGod/article/details/82149549)
- [wiki官方手册](https://www.cwiki.us/collector/pages.action?key=CONFLUENCEWIKI)
