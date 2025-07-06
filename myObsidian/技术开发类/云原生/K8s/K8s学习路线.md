
## 一、基础阶段
1. ​**​核心概念理解​**​
    - ​**​学习目标​**​：掌握Pod、Deployment、Service、ConfigMap、Secret等核心概念


## 二、实践阶段
 1. kubectl与YAML实战
     - **核心操作：** 通过`kubectl`创建Pod、Deployment、Service，并编写YAML文件（例如定义容器镜像、端口暴露）。
     - K8s一切皆资源，声明式配置是核心理念。
2. 关键场景演练
    - **服务发现：** 通过Service暴露Pod，理解ClusterIP vs NodePort。
    - **配置与存储：** 使用ConfigMap传递环境变量，用PersistentVolume（PV）持久化数据（对比Docker Volume）。
    - **实践项目：** 部署一个多容器应用（如Nginx+Redis），模拟真实微服务场景。

## 三、进阶主题
1. 控制器与高级调度
    - **核心组件：** 深入学习Deployment（滚动更新）、StatefulSet（有状态应用）、DaemonSet（节点守护进程）。
    - **为何有限学这些：** StatefulSet解决Docker单机无法处理的有状态服务（如数据库），是K8s独特价值。
2. **运维与安全​**​
    - ​**​自动扩缩​**​：配置HPA（Horizontal Pod Autoscaler），根据CPU负载动态扩缩。
    - ​**​安全加固​**​：学习RBAC（基于角色的权限控制）和ServiceAccount，避免集群未授权访问。
3. ​**​监控与日志​**​
    - ​**​工具链​**​：Prometheus（指标收集）+Grafana（可视化），ELK日志系统。
    - ​**​为何重要​**​：Docker日志分散，K8s需集中式监控才能管理大规模应用。

### **四、生产级实战（持续进行）​**​

1. ​**​云环境迁移​**​
    - ​**​平台选择​**​：在AWS EKS、Google GKE或阿里云ACK部署集群，学习Ingress控制器、负载均衡器集成。
    - ​**​为何不用纯本地环境​**​：生产需求涉及网络策略、存储类动态供应，云平台提供开箱即用支持。
2. ​**​CI/CD流水线集成​**​
    - ​**​工具示例​**​：Jenkins/GitLab CI构建Docker镜像，推送至镜像仓库，K8s自动拉取更新。
    - ​**​协同价值​**​：你的Docker技能在此直接复用，镜像构建后由K8s编排，形成完整DevOps闭环。