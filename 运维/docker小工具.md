# linux-command 命令大全

```sh
docker search linux-command
docker pull wcjiang/linux-command
docker run -d  --name linux-command --restart always  -p 6868:3000 wcjiang/linux-command:latest
```

访问 http://localhost:6868/ 



# it-tools 前端开发者工具

```sh
docker search it-tools
docker pull corentinth/it-tools
docker run -d -p 8080:80 --name it-tools -it corentinth/it-tools
```



# reference 开发人员快速参考备忘清单【速查表】

```sh
docker search reference
docker pull wcjiang/reference

# 临时的停止的时候销毁
docker run --name reference --rm -d -p 9667:3000 wcjiang/reference:latest
# Or
docker run --name reference -itd -p 9667:3000 wcjiang/reference:latest
```

