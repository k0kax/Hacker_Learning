#### 1.容器--->镜像
```docker
ocker commit vulfocus（容器名） new-image:v1.0（镜像）:(tag)
```

例如生成镜像：`docker commit -a “koka” -m “halo” b18181a154f3（或容器名） halo:v1.0`
将容器变为镜像，基本文件内容不变，不过一些元数据和映射关系（如）
#### 2.镜像--->tar
```docker
docker save -o 路径/image.tar（包名） image-name:tag（镜像名）
```
例如`docker save -o 路径/vulf.tar vulf`
#### 3.容器 --->tar

```docker
docker export -o 路径/container.tar（容器包名） container-id（容器名）
```
#### 4.镜像包导入
```docker
docker load -i 路径/image.tar
```
例如`docker load -i /path/to/vulf.tar`

#### 5.进入容器查看
`docker exec -it container-id(或容器名) /bin/bash`
