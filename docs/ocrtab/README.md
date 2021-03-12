# OCR Tab
> 从OCR 结果提炼数据

## 测试页面
> [点此访问](http://ec2-52-83-78-143.cn-northwest-1.compute.amazonaws.com.cn:8050/static/html/test_ocr.html)测试页面， 如果没有权限请找领星生物的人员帮您开通

![ocr test page preview](imgs/ocr_test_page.png)

## OCR 接口
* 访问路径 ```http://host:port/ocr```
* 请求方式 ```POST```
* Payload 数据种类 ```application/json```
* Payload 数据格式, 其中字符串值为JPEG图片数据的base64编码
```json
{"images":["/9j/4AAQS..."]}
```
* 返回格式
```json
{
    "msg":"",
    "result":[
        [
            {
                "confidence":0.9909,
                "text":"XX医院化验单",
                "text_region":[
                    [34,104],
                    [138,107],
                    [138,139],
                    [34,137]
                ]
            },
            {
                "confidence":0.991,
                "text":"生化室",
                "text_region":[
                    [14,154],
                    [239,217],
                    [237,259],
                    [32,27]
                ]
            },
            ...
        ]],
    "status":"0"
}
```