## 前言
- 开发初衷是想把自己在运维领域所积累十余年的创新实践经验，以开源的方式真正解决运维人的痛点，让运维工作可以更简单与高效。
## 平台介绍
- 平台是集轻量级、聚合型、智能运维为一体的综合管理平台，具备纳管、监控、巡检、自愈、支持异构网络环境、秒级执行运维作业、多云管理，运维工单、k8s集群管理,webshell,安全审计,堡垒机等功能并整合deepseek和ollama人工智能大模型，可为用户提供智能的运维能力和业务管理，在提高运维人员等工作效率的同时，极大提升了业务的连续性和安全性。
- 平台提供全生命周期的自动化运维工具体系，提供易用的操作界面和清晰的运维管理流程，可以快速对接企业已经使用的开源工具，避免推倒重来的重复性建设，降低从自动化到智能化运维的建设成本，提高运维管理效率，保障业务连续性。
- 平台采用模块化开发、容器部署的架构模型，前端基于vue-element-admin框架基础上进行开发，后端基于go语言开发，平台框架采用abs模式。
- demo地址：http://115.190.10.126  用户名:guest   密码: Opsone1234

## 环境依赖 
- 平台采用容器化部署故依赖k8s环境，请自行部署k8s集群环境。
- 管控服务器系统仅限linux，交换机仅限华为和H3C。
- mysql因需要数据持久化，默认会强制部署到标签为node-app:mysql的node节点，请指定1台node设置相应标签
- 容器实例mysql、redis、influxdb存储数据默认采用本地目录持久化方案，多node部署需自行修改为nfs或者其他分布式文件系统 
## 快速安装
- 在 K8S master执行下面命令即可
```
kubectl label nodes 'your-node-name' node-app=mysql  #命令设置某个node标签为node-app:mysql
kubectl create namespace opsone
kubectl apply -f https://raw.githubusercontent.com/wylok/opsone/main/opsone.yaml
kubectl apply -f https://raw.githubusercontent.com/wylok/opsone/main/metrics-server.yaml
```
- 修改configMap中的opsone-config文件，修改config.ini对应的your-node-ip:30800并保存
重新启动opsone-server容器
- 如果您想要定制 opsone的启动参数，请将该 YAML 文件下载到本地，并修改其中的ConfigMap
- 执行指令 watch kubectl get pods -n opsone，等待 opsone名称空间中所有的 Pod 就绪
```
配置服务器自动发现需要root账号,如无法使用root也可手动在被管理服务器上执行:
curl -s http://your-node-ip:30800/api/v1/ag/install.sh|bash
```
## 简单使用
- 在浏览器中打开链接 http://your-node-ip:30800 
- 输入初始用户名和密码，并登录
- 用户名： admin 
- 密码： Opsone1234
## 最佳实践
- 修改ConfigMap中opsone-config的config.ini对应的your-node-ip为node内网IP地址，保证需要管理的服务器与该IP地址可以互通，如果是异构网络建议配置为公网IP地址。
- 按照平台菜单自上而下的顺序进行配置，创建部门-->业务组-->IDC池-->ssh密钥-->资源池-->资源组-->堡垒机-->监控规则-->多云管理-->密钥管理-->工作工单-->工单流程-->平台配置-->agent管理-->消息中心
- 配置资源池需注意，后台是按照ip段数量并发执行，为了保证自动发现效率单IP段内IP数量不要超过50
## 详细教程
- https://zhuanlan.zhihu.com/p/701177000
- ![image](https://github.com/user-attachments/assets/594d9fce-e137-4cf6-9154-95cb3abe663a)

