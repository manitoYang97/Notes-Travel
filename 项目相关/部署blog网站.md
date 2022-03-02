### blog 开发部署文档

#### 部署

1. 进入服务器

```bash
ssh -i squids-ali.pem root@121.43.61.62 blog
```

2. 在服务器上下载docker、docker-compose、nginx程序


```bash
apt-get update

sudo apt-get install -y docker.io
sudo apt install docker-compose
sudo apt install nginx

```

3. 设置nginx和docker-compose开机启动
   
```
 systemctl enable docker
 
```

4. 在指定目录下添加配置文件

5. 进入mysql添加数据库

```bash
docker exec -it mysql bash  # mysql 是数据库容器名

```

6. 在数据库里建库建用户，把之前的SQL文件到进去