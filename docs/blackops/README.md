# 脱敏API 文档
> 一个贴条脱敏 API

## aws 部署
> 前提条件
* 有aws 中国的账号
* 安装好较新版本的aws-cli
* 设置好相应的```~/.aws/credential```文件和```~/.aws/config```文件, 见[aws-cli快速配置](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-config)
* docker

### 获得ECR镜像
* 获取ecr登陆密码
```shell
TOKEN=$(aws ecr get-login-password --region cn-northwest-1)
```
* 查看一下```$TOKEN``` 变量是否会返回一长串密码
```shell
echo $TOKEN
```
* docker 登陆ecr地址:
```shell
docker login -u AWS -p $TOKEN https://046687694157.dkr.ecr.cn-northwest-1.amazonaws.com.cn
```
* 获取镜像
```shell
docker pull 046687694157.dkr.ecr.cn-northwest-1.amazonaws.com.cn/blackops:latest
```