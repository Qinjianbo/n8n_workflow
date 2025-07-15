# 使用docker安装n8n

> 关于docker的安装请参考docker官网：https://docs.docker.com/get-docker/。

> 这边文章主要介绍我本地是windows系统中，使用docker安装n8n的步骤。

> 其他安装方式参考官方文档：https://docs.n8n.io/choose-n8n/。

## 1. 创建网络

> 便于后面扩展对其他服务的访问，这边先创建一个网络。

```bash
docker network create n8n-net
```

## 2. 创建数据卷

> 这边创建一个数据卷，用于挂载n8n的数据。

```bash
docker volume create n8n_data
```

## 3. 创建n8n容器

- linux/macos 请执行

```bash
docker run -it --rm \
  # 容器名称
  --name n8n \
  # 使用创建的网络
  --network n8n-net \
  # 端口映射,5678为n8n默认端口
  -p 5678:5678 \
  # 启用runners功能
  -e N8N_RUNNERS_ENABLED=true \
  # 强制设置文件权限
  -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true \
  # 设置时区为上海
  -e TZ=Asia/Shanghai \
  # 挂载n8n数据卷，便于数据持久化
  -v n8n_data:/home/node/.n8n \
  # 挂载本地工作目录，用于下载一些东西能及时同步到宿主机，方便后续使用
  # 这边我挂载的是I盘的working目录，你可以挂载到其他盘，只要能访问到就行
  -v I:/working:/home/working \
  # n8n镜像
  docker.n8n.io/n8nio/n8n
```

- windows cmd(推荐) 请执行

```bash
# windows cmd 不方便换行，注释就不写了
docker run -it --rm --name n8n --network n8n-net -p 5678:5678 -e N8N_RUNNERS_ENABLED=true -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true -e TZ=Asia/Shanghai -v n8n_data:/home/node/.n8n -v I:/working:/home/working docker.n8n.io/n8nio/n8n
```

> 执行完上面几步后，docker中就启动了n8n容器，接下来我们就可以访问n8n了。

> 访问n8n的地址为：http://localhost:5678