# LIDAR MAPPING 使用说明

### 系统要求需求
#### 硬件要求
`CPU` Intel 9 Gen Core i7 3.6GHz(至少)
`RAM` 64GB
`Graphic Card` NVIDIA RTX2060
`Disk` 500GB（至少）
#### 软件要求
ubuntu 18.04
[docker 19.03](https://docs.docker.com/engine/install/ubuntu/)
[NVIDIA docker](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)

### 拉取镜像
```shell
docker pull 172.16.4.102:9020/lidar_mapping/lidar_mapping:latest
```

### 打开镜像
```shell
docker run -it --net=host --gpus all --name lidar_mapping -v $HOME/shared_dir:/home/lidar_mapping/data 172.16.4.102:9020/lidar_mapping/lidar_mapping:latest /bin/zsh
```
### 进入container
如果需要多个terminal窗口进行操作的话，可以通过一下命令进入container
```shell
docker exec -it lidar_mapping /bin/zsh
```
### 修改配置文件
可在里面配置点云及IMU的topic，点云及IMU参数
```shell
rosed lio_sam_6axis params_vlp.yaml
```

### 运行文件
```shell
source ~/workspace/install/setup.zsh
roslaunch lio_sam_6axis run.launch
```
### 播放rosbag
```shell
rosbag play -r 3 \ # 3倍速播放
  --clock \ # 使用bag内部时间
  [file path] # bag的地址
```
### 保存地图文件
当bag播放完成后已经生成的地图，需要保存地图文件到指定目录
```bash
rosservice call /lio_sam_6axis/save_map 0.0 "/Downloads/map/" # 第一个参数的分辨率，第二个参数为保存地址
```