## 前言
- 开发初衷是想把自己在运维领域所积累十余年的创新实践经验，以开源的方式真正解决运维人的痛点，让运维工作可以更简单与高效。
## 平台介绍
- 平台是集轻量级、聚合型、智能运维为一体的智能管理平台，具备纳管、监控、巡检、自愈、支持异构网络环境、秒级执行运维作业、多云管理，运维工单、k8s集群管理,webshell,安全审计,堡垒机等功能。
- 平台提供全生命周期的自动化运维工具体系，提供易用的操作界面和清晰的运维管理流程，降低从自动化到智能化运维的建设成本，提高运维管理效率，保障业务连续性。
- 平台采用模块化开发、容器部署的架构模型，前端基于vue-element-admin框架，后端使用go语言开发。
- demo地址：http://115.190.10.126  用户名:guest   密码: Opsone1234

## 环境依赖 
- 平台采用容器化部署故依赖k8s或docker环境，请自行部署相关运行环境。
- 管控服务器系统不支持windows，交换机仅限思科、华为和H3C。
- mysql因需要数据持久化，默认会强制部署到标签为node-app:mysql的node节点，请指定1台node设置相应标签
- 容器实例mysql、redis、influxdb存储数据默认采用本地目录持久化方案
- 更多解决方案请参考平台文档:  https://zhuanlan.zhihu.com/p/701177000
## 快速安装
K8S环境部署
```
kubectl label nodes 'your-node-name' node-app=mysql  #命令设置某个node标签为node-app:mysql
kubectl create namespace opsone
kubectl apply -f https://raw.githubusercontent.com/wylok/opsone/main/opsone.yaml
kubectl apply -f https://raw.githubusercontent.com/wylok/opsone/main/metrics-server.yaml
```
- 修改configMap中的opsone-config文件，修改config.ini对应的your-node-ip:30800并保存
重新启动opsone-server容器
- 执行指令 watch kubectl get pods -n opsone，等待 opsone名称空间中所有的 Pod 就绪
- 如果非root账号选择手动安装agent,在被管理服务器上执行:curl -s http://your-node-ip:30800/api/v1/ag/install.sh|bash

Docker环境部署
```
wget https://raw.githubusercontent.com/wylok/opsone/main/docker-opsone.tgz
tar -zxvf docker-opsone.tgz
cd opsone
修改config.ini对应的ip为所部署服务器真实ip
sh start_up.sh
停止opsone平台服务在opsone目录下执行docker-compose down
```
- 不要修改解压缩后opsone文件夹的名称
- docker版需要用到docker-compose，请自行安装
- 如果非root账号选择手动安装agent,在被管理服务器上执行:curl -s http://your-host-ip/api/v1/ag/install.sh|bash

## 简单使用
- k8s版：在浏览器中打开链接 http://your-node-ip:30800
- docker版：在浏览器中打开链接 http://your-host-ip
- 输入初始用户名和密码，并登录
- 用户名： admin 
- 密码： Opsone1234
## 最佳实践
- 按照平台菜单自上而下的顺序进行配置，创建部门-->业务组-->IDC池-->ssh密钥-->资源池-->资源组-->堡垒机-->监控规则-->多云管理-->密钥管理-->工作工单-->工单流程-->平台管理-->安全审计
- 配置资源池需注意，后台是按照ip段数量并发执行，为了保证自动发现效率单IP段内IP数量不要超过50
## 详细教程
- https://zhuanlan.zhihu.com/p/701177000
## 加入微信群
- ![image](https://raw.githubusercontent.com/wylok/opsone/main/g1.jpg)

