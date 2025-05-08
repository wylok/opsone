## 前言
- 开发初衷是想把自己在运维领域所积累十余年的创新实践经验，用轻量化一站式智能运维平台的方式解决运维日常工作痛点问题，让运维工作可以更快速、简单与高效
- ![image](https://raw.githubusercontent.com/wylok/opsone/main/images/login.jpg)
## 平台介绍
- 平台是集轻量化、极简操作、高效运维为一体的智能运维平台，具备纳管、监控、巡检、自愈、支持异构网络环境、秒级执行运维作业、多云管理，运维工单、k8s集群管理,webshell,安全审计,堡垒机等功能。
- 平台提供全生命周期的自动化运维工具体系，提供易用的操作界面和清晰的运维管理流程，降低从自动化到智能化运维的建设成本，提高运维管理效率，保障业务连续性。
- 平台采用模块化开发、容器部署的架构模型，前端基于vue-element-admin框架，后端使用go语言开发。
- demo地址：http://115.190.10.126  用户名:guest   密码: Opsone1234
- 平台网站: https://wylok.github.io/opsone/

## 环境依赖 
- 平台采用容器化部署故依赖k8s或docker环境，请自行部署相关运行环境。
- 服务器操作系统不支持windows，交换机仅限思科、华为和H3C。
- mysql因需要数据持久化，默认会强制部署到标签为node-app:mysql的node节点，请指定1台node设置相应标签
- 容器实例mysql、influxdb存储数据默认采用本地目录持久化方案
- 更多解决方案请参考平台文档:  https://zhuanlan.zhihu.com/p/701177000
## 快速安装
K8S环境部署
```
kubectl label nodes 'your-node-name' node-app=mysql  #命令设置某个node标签为node-app:mysql
kubectl create namespace opsone
kubectl apply -f https://raw.githubusercontent.com/wylok/opsone/main/k8s/opsone.yaml
kubectl apply -f https://raw.githubusercontent.com/wylok/opsone/main/k8s/metrics-server.yaml
```
- 修改configMap中的opsone-config文件，修改config.ini对应的your-node-ip:30800并保存
- 重新启动opsone-server容器
- 执行指令 watch kubectl get pods -n opsone，等待 opsone名称空间中所有的 Pod 就绪
- 如果非root账号选择手动安装agent,在被管理服务器上执行:curl -s http://your-node-ip:30800/api/v1/ag/install.sh|bash

Docker环境部署
```
wget https://raw.githubusercontent.com/wylok/opsone/main/docker/docker-opsone.tgz
tar -zxvf docker-opsone.tgz
cd opsone
修改config.ini对应的your-host-ip为所部署服务器真实ip
sh start_up.sh
停止opsone平台服务在opsone目录下执行docker-compose down
```
- docker版需要用到docker-compose，请自行安装
- 如果非root账号选择手动安装agent,在被管理服务器上执行:curl -s http://your-host-ip/api/v1/ag/install.sh|bash

## 简单使用
- k8s版：在浏览器中打开链接 http://your-node-ip:30800
- docker版：在浏览器中打开链接 http://your-host-ip
- 输入初始用户名和密码，并登录
- 用户名： admin 
- 密码： Opsone1234
- 平台prometheus监控接口：/api/v1/metrics
- 
## 开源说明
- 本智能运维平台开源代码遵循GPL-3.0开源协议,可免费运行软件,可任意修改代码,可免费分发原版或修改后的代码,但是必须公开全部源代码并同样以GPL-3.0 或兼容许可证发布.
- 未经原作者授权禁止闭源将代码转为私有商业产品.
- 平台后续新增功能及代码优化，bug修复不保证会及时进行开源代码更新.
- 本次开源代码不包含webshell录屏回放以及k8s集群pod监控报警等安全审计功能.

