#### 1.容器--->镜像
```docker
ocker commit vulfocus（容器名） new-image:v1.0（镜像）:(tag)
```

例如生成镜像：`docker commit -a “koka” -m “halo” b18181a154f3（或容器名） halo:v1.0`

#### 2.镜像--->tar
```docker
docker save -o image.tar（包名） image-name:tag（镜像名）
```
例如`docker save -o l/vulf.tar vulf`
#### 3.容器 --->tar


容器打包：docker export -o container.tar（容器包名） container-id（容器名）

导入镜像：docker load -i 地址/image.tar

进入容器内部查看并运行shell: docker exec -it container-id(或容器名) /bin/bash
