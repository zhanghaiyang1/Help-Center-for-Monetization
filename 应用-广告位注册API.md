# 1. 接口接入说明

### 1.1 接口地址

若您在中国大陆，请使用 https://pa-developer.zplayads.com

若您在中国大陆以外地区，请使用 https://pa-developer-en.zplayads.com

### 1.2 接入条件
* 确保账号已经审核通过
* 需要相关的Account ID和API Key，请登录ZPLAY Ads系统后点击左侧导航的个人信息，在页面的底部进行查看

### 1.3 接口风格
* 采用restfull api风格
* 以http状态码来表明请求资源的状态

### 1.4 接口限制
* 采用签名验证的方式来验证权限

### 1.5 接口的验证
开发者审核通过后,请求接口必须带有三个参数

参数 | 类型 | 位置 | 描述
---| -- | --- | --
signature | string | query | 加密签名，signature结合了开发者的API Key和请求中的timestamp参数、nonce参数。
timestamp | int | query | 时间戳，比如：1534305711，[参考地址](https://tool.lu/timestamp/)。
nonce | int | query | 随机数，必须是正整数。

**加密/校验流程如下：**
1. 将API Key、timestamp、nonce三个参数进行字典序排序
2. 将三个参数字符串拼接成一个字符串进行sha1加密
3. 开发者获得加密后的字符串可与signature对比，标识该请求合法

*检验signature的PHP示例代码：*

```php
private function checkSignature()
{
  $signature = $_GET["signature"];
  $timestamp = $_GET["timestamp"];
  $nonce = $_GET["nonce"];  

  $token = api key; // 登录ZPLAY Ads系统后点击左侧导航的个人信息，在页面的底部找到API Key
  $tmpArr = array($token, $timestamp, $nonce);
  sort($tmpArr, SORT_STRING);
  $tmpStr = implode( $tmpArr );
  $tmpStr = sha1( $tmpStr );

  if ($tmpStr == $signature) {
    return true;
  } else {
    return false;
  }
}
```

### 1.6 响应的数据格式
* 数据格式为json
* 字段说明
    * error 错误信息,当请求有误时的提示文字
    * data 数据
  
### 1.7 常见的http状态码释义

http 状态码 | 释义
---|---
200 | 获取成功
400 | 请求参数有误，请检查参数
401 | 用户验证有误，请检查验证信息
500 | 后端服务错误


# 2. 新增应用

### 2.1 接口信息

url | method | 说明
---|---|--
/api/developer/apps | POST | 新增应用

### 2.2 请求参数

字段 | 数据类型 | 位置 | 是否 | 必填说明
---|---|--|--|--
developer_account_id | string | form | 是 | 开发者id
os					 | string | form | 是 | ios/android 
name | string | form | 是 | 100字符以内
package_name | string | form | 是 | 100字符以内
category | string | form | 否 | 若填值，必须是指定的类型，参照第4部分英文
download_url | string | form | 否 | 若填值，必须是http/https链接
app_store | int | form | 是 | 必须是以下列列表中的值，参照第5部分对应id



### 2.3 响应
#### 示例

```json
{
    "error": "",
    "data": 
        {
            "guid": "80183BCF-6B19-0C8D-AC00-112DA4AEF846"
        }
}
```

# 3. 添加广告位

### 3.1 接口信息

url | method | 说明
---|---|--
/api/developer/adplace | POST | 新增广告位

### 3.2 请求参数

字段 | 数据类型 | 位置 | 是否 | 必填说明
---|---|--|--|--
developer_account_id | string | form | 是 | 开发者id
name | string | form | 是 | 100字符以内
app_id					 | string | form | 是 | 所属应用id
ad_type					 | int | form | 是 | 需为系统中的广告形式，1：插屏，3：原生，4：激励视频，需要校验接口中的值，值不正确时注册失败
can_skiped | int | form | 否 | 当广告形式为激励视频时，此字段必填，0：不可跳过，1：可跳过，需要校验接口中的值，值不正确时注册失败
render_method | int | form | 否 | 当广告形式为原生时，此字段必填，1：托管渲染，2：自渲染，需要校验接口中的值，值不正确时注册失败
render_style | string | form | 否 | 当广告形式为原生时，此字段必填，值为：DF0641C3-B89B-AE2E-52E3-33E89ADB6BC1，需要校验接口中的值，值不正确时注册失败
remark | string | form | 否 | 500字符以内，一个汉字/字母/标点符号算作一个字符



### 3.3 响应
#### 示例

```json
{
    "error": "",
    "data": 
        {
            "guid": "80183BCF-6B19-0C8D-AC00-112DA4AEF846"
        }
}
```

# 4. 应用分类

### 4.1 列表
应用类别 | Category 
---|---
动作游戏|Action
益智解谜|Puzzle
卡牌游戏|Card
休闲|Casual
冒险游戏|Adventure
角色扮演游戏|Role-playing
策略游戏|Strategy game
街机游戏|Arcade
儿童|Kids
竞速游戏|Racing
聚会游戏|Family
模拟游戏|Simulation
体育|Sports
文字游戏|Word
问答游戏|Trivia
音乐|Music
桌面游戏|Board
赌场|Casino
教育|Education

# 5. 应用商店

### 5.1 列表
应用商店 | Store 
---|---
50 | App Store
1 | Google play
2 | Application of Treasure
3 | Baidu Handset Assistant
4 | 360 Handset Assistant
5 | Wandoujia
6 | Kupai Application Store
7 | Anzhi
8 | HUAWEI Application Store
9 | 91 Assistants
10 | 10086
11 | PP Assistants
12 | UC Game Center
13 | 189 Store
14 | GFan
15 | Zhuoyi Market
16 | Mi
17 | MEIZU Application Store
18 | Lenovo Mobile Market
19 | Jinli Software Store
20 | OPPO Software Store
21 | Jinli Game Center
22 | VIVO
26 | Others

