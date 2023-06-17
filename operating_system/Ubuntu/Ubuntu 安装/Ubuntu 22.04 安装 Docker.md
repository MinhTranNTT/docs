## 安装基础工具

```shell
sudo apt-get install ca-certificates curl gnupg lsb-release
```

## 安装官方的GPG key

```shell
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

## 将Docker的库添加到apt资源列表中

```shell
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## 更新apt

```shell
 sudo apt update
```

## 安装 Docker 官方版

```shell
 sudo apt install docker-ce
```

‍
