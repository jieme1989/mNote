dotnet网址

[从.NET下载sdk和运行环境](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)

[下载sdk linux  arm32](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/sdk-8.0.404-linux-arm32-binaries )

[下载runtime  linux arm32](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-aspnetcore-8.0.11-linux-arm32-binaries)

注意选择版本

[下载runtime linux arm64](https://download.visualstudio.microsoft.com/download/pr/64a9f696-b039-4a73-b705-288fbf9c2e8f/c36bc24d6d359c019408b4f94ee67b59/aspnetcore-runtime-8.0.11-linux-arm64.tar.gz)

[下载sdk linux arm64](https://download.visualstudio.microsoft.com/download/pr/5ac82fcb-c260-4c46-b62f-8cde2ddfc625/feb12fc704a476ea2227c57c81d18cdf/dotnet-sdk-8.0.404-linux-arm64.tar.gz)

解压下载的两个文件

```sh
cd ~
mkdir -p dotnet
tar -zxf dotnet-sdk-*****.tar.gz -C ./dotnet/
tar -zxf aspnetcore-runtime-*****.tar.gz -C ./dotnet/
```

把 dotnet 文件夹移动到 /usr/share/ 目录下

```sh
sudo mv dotnet /usr/share/dotnet/
```

移动过去之后，再来创建个软连接

```sh
sudo ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
```

重启下终端或者ssh

```sh
dotnet --version
```

