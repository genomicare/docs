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
* 获取镜像，大概4g多一点
```shell
docker pull 046687694157.dkr.ecr.cn-northwest-1.amazonaws.com.cn/blackops:latest
```

### 运行docker
```shell
docker run -t -d --rm -dp 8867:8867 --name blackops 046687694157.dkr.ecr.cn-northwest-1.amazonaws.com.cn/blackops
```
当然， 在运行这一步的时候， 您可以更改映射的外部端口号， 在使用接口时访问同样的端口号即可

### 运行服务
```shell
docker exec -it blackops /bin/bash taping_gunicorn.sh 10
```
* 这样就能在 http://[host]:8867 看到服务了

## 服务调用

### 测试页面
如果想要比较直观的体验

可以进入```http://[host]:8867/static/test.html```测试， 页面上会打印出调用接口时实际传入的参数

### 接口访问
如果直接访问接口使用

* 地址 ```/taping```
* 请求方法 ```POST```
* 请求headers
```json
{"Content-Type": "application/json"}
```
* 数据传入json字符串

#### 输入数据字段解释
以下是传入数据的案例
```json
{
    "block_name": false,
    "block_value": true,
    "box_distance": 0.15,
    "color": null,
    "return_input": true,
    "return_json": true,
    "mask_keys": [
        "姓名",
        "住院"
    ],
    "images": [
        "/9j/4AAQSkZJRgABAQAA..."
    ]
}
```
* block_name, bool, 是否屏蔽字段名（中关键词的bounding box 本身)
* block_value, bool, 是否屏蔽字段值 (中关键词的bounding box 右侧的bounding box， 两者相距不超过图片宽度X box_distance)
* box_distance, float, 0~1之间浮点数， 用来判断两个bounding box 是否够近
* color, string, 默认白色， 具体用来屏蔽文本的颜色， 输入色彩编码， 如 ```#AE134F```
* return_input:bool, 默认false, 是否返回输入的json
* return_json:bool, 默认false, 是否返回ocr的结果用于分析
* mask_keys: list[string], 默认["姓名", "住院"], 需要屏蔽的关键词们, 身份证不走这套规则， 走身份证独有的规则
* images: list[string], jpg图片， base64编码的bytes数据，不包含头部```data:image/jpg;base64,```， 理论上在html页面中， ```<img src='data:image/jpg;base64,{图片base64字符串}'>```可以直接显示图片

## 其他操作
### 如何加速运行
除了起更多接口之外， 我们还可以在不影响使用效果的情况下将图片resize小一号再输入，大的图片在传输，卷积运算，写屏蔽后的图片时都很耗时

### 没有屏蔽对怎么办
可能有以下几种原因
* 匹配关键词时没有匹配上
* 字段名和字段值的bounding box粘连成一个 或 太远， 比如有时候姓名 张三占据两个box, 有时候会并到一个box
* 身份证中有些字段没有符合身份证的关键词

以上几种原因都可以通过return_json查看ocr结果来分析