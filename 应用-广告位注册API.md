# 1. 应用、广告位插入说明


### 1. 添加应用

字段 | 数据类型 | 是否 | 必填说明
---|---|--|--
developer_id | string |  是 | 开发者id
os					 | string |  是 | 操作系统，ios/android 
name | string |  是 | 应用名称，100字符以内
package_name | string |  是 | 包名，100字符以内
industry_ud | string |  是 | 行业id，必须是指定的类型，参照第3部分英文，根据英文转换成对应的id例如：36FBA1A1-C5E7-D9F6-9170-9BA0DA13CBF9
app_store | int |  是 | 应用商店，必须是以下列列表中的值，参照第4部分对应id，存储id值
im_app_id | string |  是 | im应用id
package_filter| string |  是 | 黑名单、取自己的包名



# 2. 添加广告位




字段 | 数据类型 | 是否 | 必填说明
---|---|--|--
developer_id | string |  是 | 开发者id
name | string |  是 | 广告位名称，100字符以内
app_id					 | string |  是 | 所属应用id
ad_type					 | int |  是 | 广告类型，需为系统中的广告形式，1：插屏，3：原生，4：激励视频，需要校验接口中的值，值不正确时注册失败
package_filter	 | string |  是 | 	所属应用的包名
render_method | int |  否 | 渲染方式，当广告形式为原生时，此字段必填，1：托管渲染，2：自渲染，需要校验接口中的值，值不正确时注册失败
render_style | string |  否 | 渲染样式，当广告形式为原生时，此字段必填，值为：DF0641C3-B89B-AE2E-52E3-33E89ADB6BC1，需要校验接口中的值，值不正确时注册失败




# 3. 应用分类

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

# 4. 应用商店

### 5.1 列表
应用商店 | Store | 中文
---|--- | ---
50 | App Store | 苹果
1 | Google play	 | 谷歌
2 | Application of Treasure | 应用宝
3 | Baidu Handset Assistant | 百度应用商店
4 | 360 Handset Assistant | 360手机助手
5 | Wandoujia | 豌豆荚
6 | Kupai Application Store | 酷派应用商店
7 | Anzhi | 安智
8 | HUAWEI Application Store | 华为应用市场
9 | 91 Assistants | 91助手
10 | 10086 | 移动MM
11 | PP Assistants | PP助手
12 | UC Game Center | UC游戏中心
13 | 189 Store | 天翼空间
14 | GFan | 机锋市场
15 | Zhuoyi Market | 卓易市场
16 | Mi | 小米应用商店
17 | MEIZU Application Store | 魅族应用商店
18 | Lenovo Mobile Market | 联想乐商店
19 | Jinli Software Store | 金立软件商店
20 | OPPO Software Store | OPPO软件商店
21 | Jinli Game Center | 金立游戏大厅
22 | VIVO | VIVO
26 | Others | 其它

