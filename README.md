ceph一键部署脚本docker（luminous）

# 根据需要配置相应的参数

脚本主要功能（可选）：

自动互信，

时钟同步，

更改主机名，

自动部署mon集群，

自动部署OSD集群，

自动部署MGR主备

自动部署RGW集群

自动添加portainer监控，管理集群容器

# 使用步骤：
cd deploy_ceph(luminous)

chmod -R 755 .

编辑base.config里面的参数

./deploy_ceph_master.sh

执行完后刷新所在服务器环境变量，或者重新登录服务器。

执行ceph -s查看集群情况

MGR集群监控情况，根据集群显示结果查看MGR位置，并输入相应的IP及端口号

# 添加OSD命令（替换相应变量）

#$ceph_base_path 磁盘设备根目录

#$odisk 磁盘设备名

#$ceph_base_path ceph基础目录

# 擦盘

docker run --rm —privileged=true \
-v $disk_path/:/dev/ \
-e OSD_DEVICE=$disk_path/$odisk \
registry.cn-hangzhou.aliyuncs.com/yangb/ceph_luminous (http://registry.cn-hangzhou.aliyuncs.com/yangb/ceph_luminous) zap_device

# 添加OSD

docker run -d --net=host --name=$odisk —privileged=true \
-v $ceph_base_path/etc/:/etc/ceph \
-v $ceph_base_path/lib/:/var/lib/ceph \
-v $disk_path/:/dev/ \
-e OSD_DEVICE=$disk_path/$odisk \
-e OSD_TYPE=disk \
-e CLUSTER=ceph registry.cn-hangzhou.aliyuncs.com/yangb/ceph_luminous (http://registry.cn-hangzhou.aliyuncs.com/yangb/ceph_luminous) osd_ceph_disk

自动部署了容器管理工具portainer，可以访问部署节点的9000端口查看和管理各个节点的容器运行情况

欢迎大家关注我个人的订阅号，会定期分享学习心得，相关案例信息!

![index1](https://github.com/luckman666/devops_kkit/blob/master/gzh.jpg)

