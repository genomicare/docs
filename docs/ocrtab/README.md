# OCR Tab
> 从OCR 结果提炼数据

## 测试页面
> [点此访问](http://ec2-52-83-78-143.cn-northwest-1.compute.amazonaws.com.cn:8050/static/html/test_ocr.html)测试页面， 如果没有权限请找领星生物的人员帮您开通

![ocr test page preview](imgs/ocr_test_page.png)

## OCR 接口
* 访问路径 ```http://[host]:[port]/ocr```
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
### 获得的Bouding box
一般OCR接口能返回一堆Bounding box，其中一个box案例如下
```json
{
                "confidence":0.9909,
                "text":"XX医院化验单",
                "text_region":[
                    [34,104],
                    [138,107],
                    [138,139],
                    [34,137]
                ]
}
```

* text: box中的文本
* text_region: box四个角的x 和y的像素坐标， 如左上角的坐标即是```(从左到右第34个像素, 从上到下第104个像素)```
* confidence: 置信度， 0~1之间的浮点数， 数字越大置信度越高

## 信息的提取和筛选
在OCR完成后， 您能获得的仅仅是一堆有x, y 坐标的散乱的Bounding box, 而许多项目往往需要更精确的筛选和提取。
![bounding boxes in yellow](imgs/ocrtab1.png)

### 案例：比如身份证的OCR结果中，您想要提取住址
![id example](imgs/id_address.png)

### 为什么不能根据关键词筛选需要的信息？
多数时候， 您无法使用关键词筛选， ，但是实际住址的信息不包含“住址”两个字， 无法确定住址信息里一定包含什么词， 而且这一例中的住址有两个框， 肯定有一个不包含“省”和“市”。 

### 为什么不通过x,y 位置直接定位到我们要的结果？
扫描时把身份证放入扫描窗口的左上角还是右下角对结果的影响很大

### 一个比较靠谱的解决方案
我们可以依靠bounding box们和周边关键词进行筛选， 比如:
* 我们可以确定这个信息在```"住址"```的右边，
* ```"出生"```的下方，
* 如果扫描质量够好， 还能隐约读出CHINA， 那我们就按照```"INA"```来算， 同时在```"公明身份证号"```和```"INA"```的上方

无论身份证的位置怎么动， 都能自动定位到我们想要的信息， 只要身份证格式不变， 这样的任务就能大规模自动化🚀

![filter result](imgs/id_address2.png)

过滤后只剩三段文本，直接衔接起来就是我们想要的结果

### 接口细节

* 访问路径 ```http://[host]:[port]/filter_anchors```
* 请求方式 ```POST```
* Payload 数据种类 ```application/json```
* Payload 数据格式
```json
{
    "bboxes":[
        {...},{...},...,{...}
    ],
    "anchors":{
        "upper":"出生",
        "left":"住址",
        "right":null,
        "lower":"公明身份证号,INA"
    }
}
```
- bboxes: OCR接口返回的那堆bounding box们
- anchors: 能划出上下左右边界的关键词， 如果没有命中则不进行筛选
* 返回格式: 返回的结果会过滤掉所有在边界外的box们（包括边界）
```json
{"bboxes":[
    {...},{...}
]}
```
