# 1. 接口接入说明

### 1.1 接口地址

若您使用人民币的结算方式，请使用 https://pa-developer.zplayads.com

若您使用非人民币的结算方式，请使用 https://pa-developer-en.zplayads.com

### 1.2 接入条件
* 确保账号已经审核通过
* 需要相关的Account ID和API Key，请到ZPLAYAds开发者自助系统[个人信息](https://sellers.zplayads.com/#/personal/personalInfo/)页面下方获取您的Account ID和API Key

### 1.3 接口风格
* 采用restfull api风格
* 以http状态码来表明请求资源的状态

### 1.4 接口限制
* 采用签名验证的方式来验证权限
* 验证通过后，同一资源每天最多可请求100次

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


# 2. 添加应用

### 2.1 描述
开发者创建应用，每天最多请求100次

### 2.2 接口信息

url | method | 说明
---|---|--
/api/developer/apps | POST | 新增应用

### 2.3 请求参数

字段 | 数据类型 | 是否 | 必填说明
---|---|--|--
developer_account_id | string |  是 | Account ID，请到ZPLAYAds开发者自助系统[个人信息](https://sellers.zplayads.com/#/personal/personalInfo/)页面下方获取您的Account ID
os					 | string |  是 | 应用操作系统，值为ios，android 
name | string |  是 | 应用名称，100字符以内
package_name | string |  是 | 应用包名，100字符以内
category | string |  是 | 应用分类，需填写英文，具体值请见[应用分类列表](#5-应用分类)
download_url | string |  否 | 应用下载地址，若填值，必须是http/https链接
app_store | int |  是 | 应用商店，需填写ID，具体值请见[应用商店列表](#6-应用商店)
industry_filter | string |  否 | 该流量应用屏蔽的投放应用分类。若填值，需填写英文，具体值请见[应用分类列表](#5-应用分类)


### 2.4 响应
#### 示例

```json
{
    "error": "",
    "data": 
        {
            "app_id": "80183BCF-6B19-0C8D-AC00-112DA4AEF846"
        }
}
```

# 3. 获取应用审核状态
### 3.1 描述
获取应用的审核状态，一次最多可以获取100个应用的审核状态，每天最多请求100次

### 3.2 接口信息


# 4. 添加广告位

### 4.1 描述
开发者创建广告位，广告位一次最多可创建5个，每天最多请求100次

### 4.2 接口信息

url | method | 说明
---|---|--
/api/developer/adplace | POST | 新增广告位

### 4.3 请求参数

字段 | 数据类型 | 是否 | 必填说明
---|---|--|--
developer_account_id | string |  是 | Account ID，请到ZPLAYAds开发者自助系统[个人信息](https://sellers.zplayads.com/#/personal/personalInfo/)页面下方获取您的Account ID
name | string |  是 | 广告位名称，100字符以内
app_id	 | string |  是 | 该广告位所属应用ID，为请求新增应用接口时获取的app_id，或在ZPLAYAds[应用管理](https://sellers.zplayads.com/#/app/appList/)页面中获取应用的应用ID
ad_type					 | int |  是 | 广告类型，需为系统中的广告形式。1=插屏，3=原生，4=激励视频
can_be_skipped | int |  否 | 广告是否可以跳过。当广告形式为激励视频时，此字段必填。0=不可跳过，1=可跳过
render_method | int |  否 | 原生广告的渲染方式。当广告形式为原生时，此字段必填。1=托管渲染，2=自渲染
render_style | string |  否 | 原生广告的渲染样式。当广告形式为原生时，此字段必填，值为DF0641C3-B89B-AE2E-52E3-33E89ADB6BC1
remark | string |  否 | 备注，500字符以内

### 4.4 响应
#### 示例

```json
{
    "error": "",
    "data": 
        {
            "ad_unit_id": "80183BCF-6B19-0C8D-AC00-112DA4AEF846"
        }
}
```

# 5. 应用分类

|  ID  |  应用类别  |  Category  |
|  -  |  -  |  -  |
|  B7405162-B110-E039-0796-7D9A766E8B27  |  新闻  |  News  |
|  F682D42C-A3F1-F854-A4F1-9B95EAF82ED9  |  图书  |  Library  |
|  79CBC4E4-1C81-3D00-C90B-741E89AC294C  |  社交  |  Social  |
|  880E064C-CCB1-013B-BFD1-37AD5DEEE528  |  生活  |  Life  |
|  B37E28A3-F0D5-ECA9-419F-A87F73E899A8  |  财务  |  Finance  |
|  F2EFEBD9-290B-362B-E799-69DC3990A60C  |  娱乐  |  Entertain  |
|  1F97B2D3-9FF6-3108-027B-679B6D387E22  |  教育  |  Education  |
|  200E54DC-CFF5-EDC4-A4CD-89CE5C747C88  |  旅行  |  Travel  |
|  397242EE-2865-9420-1EF2-C71C9BD9703B  |  导航  |  Navigation  |
|  664C6DA9-D35B-A37E-37B0-CA64128C5041  |  商业  |  Business  |
|  5FBC0108-47D5-3014-1315-8416379D6AF4  |  工具  |  Tool  |
|  BEA4FCCE-2037-1596-EF75-C034DF44F69C  |  效率  |  Efficiency  |
|  7741B885-EC32-9F76-C200-B01E0AB6C9F7  |  健康健美  |  Fitness  |
|  D6641394-E658-9857-0E82-C30BA86798B5  |  摄影与录像  |  Camera  |
|  E777608D-BB45-8A2D-F64A-ECD433553BE4  |  商品指南  |  Product Guide  |
|  C15D7FA6-1AF5-76C2-19F0-71C33EEE533F  |  美食佳饮  |  Food and Drink  |
|  A295D31D-47D9-FCB9-BF38-F5C958DFDD5C  |  参考  |  Reference  |
|  993667EF-0B50-33AF-627A-B2BDFC18F149  |  报刊杂志  |  Magazine  |
|  A42AAEF7-8992-E19D-4C53-00FE406CE607  |  体育  |  Sports  |
|  C5431997-0943-5B5B-5693-F75D66481766  |  天气  |  Weather  |
|  4609676E-BA15-B635-CE6C-022CCB7E34EB  |  医疗  |  Medical Treatment  |
|  C271F038-85D6-2DD0-0AC9-8D35CA8DE316  |  音乐  |  Music  |
|  E4C887C3-8F3F-B896-70A3-AAB325FA6768  |  视频  |  Video  |
|  26039567-EDE2-2D97-BE16-477FAF639041  |  购物  |  Shopping  |
|  F015DDC1-8475-3138-37FB-D824608C16AF  |  交通  |  Traffic  |
|  AD9C1D4C-272D-7E6B-BB64-B770A8A5B9B0  |  动作游戏  |  Action Game  |
|  AED099F1-2B7E-9A52-9F2E-D4F7CFE500AA  |  益智解谜  |  Puzzle Game  |
|  7AD7776C-20D1-7A2A-9388-0D67A44FF399  |  卡牌游戏  |  Card Game  |
|  36FBA1A1-C5E7-D9F6-9170-9BA0DA13CBF9  |  休闲游戏  |  Casual Game  |
|  1CF3914A-E099-92A5-0ECF-D3C05A177253  |  冒险游戏  |  Adventure Game  |
|  40E3C001-ADF6-A950-6E73-5DB66B0648E3  |  角色扮演游戏  |  Role-playing Game  |
|  7DA00174-66E2-5119-6355-331049316283  |  策略游戏  |  Strategy Game  |
|  F3C58318-6A2E-223E-C3D5-5936C48196B1  |  街机游戏  |  Arcade Game  |
|  AEDADC6C-562B-751F-E137-907A605BAA6C  |  儿童游戏  |  Kids Game  |
|  34879F45-896A-FFA9-582E-D576345F1D9A  |  竞速游戏  |  Racing Game  |
|  C91C130E-3166-FB45-8951-25DC86850C12  |  聚会游戏  |  Family Game  |
|  E13A497E-BCFA-1E66-05E4-31B716765B5B  |  模拟游戏  |  Simulation Game  |
|  CEDC63B5-9A57-DFD0-75CB-1289DEB7578A  |  体育游戏  |  Sports Game  |
|  48EF79CA-B384-F7A8-736A-423E8D62A023  |  文字游戏  |  Word Game  |
|  7F43E937-69EA-5C4A-954B-E52B10094225  |  问答游戏  |  Trivia Game  |
|  59B07A55-A40B-0D29-BF2D-CF29AFED540E  |  音乐游戏  |  Music Game  |
|  EC5336A4-A434-BDDA-4246-6F50DA2EF63E  |  桌面游戏  |  Board Game  |
|  57728DCB-9EC5-DC08-B999-3F9DDDF66083  |  赌场游戏  |  Casino Game  |
|  CB45DD59-EF28-EC0D-423B-2ED10F54C8D7  |  教育游戏  |  Education Game  |

# 6. 应用商店

应用商店ID|	应用商店名称
---	|	 ---
50	|	 苹果
1	|	 谷歌
2	|	 应用宝
3	|	 百度应用商店
4	|	 360手机助手
5	|	 豌豆荚
6	|	 酷派应用商店
7	|	 安智
8	|	 华为应用市场
9	|	 91助手
10	|	 移动MM
11	|	 PP助手
12	|	 UC游戏中心
13	|	 天翼空间
14	|	 机锋市场
15	|	 卓易市场
16	|	 小米应用商店
17	|	 魅族应用商店
18	|	 联想乐商店
19	|	 金立软件商店
20	|	 OPPO软件商店
21	|	 金立游戏大厅
22	|	 VIVO
26	|	 其它
