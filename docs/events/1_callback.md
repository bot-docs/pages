---
sort: 1
---

# Bot接收消息

## 基本信息

**Path：** /{第三方提供的接收地址}

**Method：** POST

**接口描述：**


## 消息参数

### Headers

| 参数名称      | 参数值           | 是否必须 | 示例 | 备注 |      |
| ------------- | ---------------- | -------- | ---- | ---- | ---- |
| Content-Type  | application/json | 是       |      |      |      |

### Body

| 名称                                                      | 类型       | 是否必须 | 备注                                                         |
| ------------                                             | ---------- | -------- | :----------------------------------------------------------- |
| signal                                                   | string    | 必须     | 消息类型,1:消息 ,2:心跳;3:加群;4:退群;5:通知修改文本;6:通知修改图片                       |
| verify_token                                             | string     | 必须     | 验证token(判断消息合法性)                                   |
| heartbeat                                                | string     | 非必须     | 消息类型为2时，需原样返回                                  |
| group_info                                               | object     | 非必须   | 加群退群信息(可做加群初始化逻辑)                             |
| data                                                     | object[]   | 非必须   | 消息数据                                                     | 
| &nbsp;&nbsp;&nbsp;├ type                                 | string     | 非必须   | 消息类型 common:普通消息 ,control控制消息                    |
| &nbsp;&nbsp;&nbsp;├ scope                                | string     | 非必须   | 消息范围 channel:频道消息,private:私聊信息                   |
| &nbsp;&nbsp;&nbsp;├ l2_type                              | integer    | 非必须   | 消息内容类型 1:文本消息,2:视频消息，3:图片消息,4:文件消息,5:音频消息,6:信令消息,<br>7:富文本消息8:markdown消息,9:卡片消息,10:系统消息,11:表情消息,12混合消息,13互动消息 |
| &nbsp;&nbsp;&nbsp;├ l3_types                             | integer[]  | 非必须   | 消息包含类型 1:回复消息;2表态消息;3:at消息;4:内部链接     |
| &nbsp;&nbsp;&nbsp;├ sender_uid                           | string    | 非必须   | 发送方uid                                                    |
| &nbsp;&nbsp;&nbsp;├ msg_id                               | string     | 非必须   | 消息id                                                       |
| &nbsp;&nbsp;&nbsp;├ gid                                  | string    | 非必须   | 群组ID; 消息范围为private时为0                               |
| &nbsp;&nbsp;&nbsp;├ target_id                            |  string   | 非必须   | 目标ID; 消息范围channel:目标的频道id; 消息范围person:接收人ID |
| &nbsp;&nbsp;&nbsp;├ ts                                   | string    | 非必须   | 发送时间的毫秒时间戳                                         |
| &nbsp;&nbsp;&nbsp;├ nonce                                | string     | 非必须   | 随机串，与用户消息发送 api 中传的 nonce 保持一致             |
| &nbsp;&nbsp;&nbsp;├ body                                 | object     | 非必须   | 不同的消息不同的格式                                         |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ content            | string     | 非必须   | 文本内容                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ video_info         | object[]   | 非必须   | 视频数据                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ pic_info           | object[]   | 非必须   | 图片数据                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ file_info          | object[]   | 非必须   | 文件数据                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ audio_info         | object[]   | 非必须   | 语音数据                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ signaling_msg      | object     | 非必须   | 信令消息                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ system_msg         | object     | 非必须   | 系统消息                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ sticker_msg        | object     | 非必须   | 表情消息                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ mixed_msg          | object     | 非必须   | 混合消息,目前只用于存储顺序                                  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ reply_msg          | object     | 非必须   | 回复消息                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ at_msg             | object     | 非必须   | at消息                                                       |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ card_info          | object[]   | 非必须   | 卡片消息                                                     |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ interaction_msg    | object[]   | 非必须   | 互动类消息                                                   |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├ bot_data           | object     | 非必须   | 机器人相关数据                                               |

### 响应

| 名称  | 类型      | 备注              |
|-----|---------|-----------------|
| ret | integer | 返回码,0:成功 非0:错误码 |
| msg | string  | 结果              |

#### 示例

```json
{
	"ret": 0,
	"msg": "ok"
}

```

### l2_type说明

#### 1.文本消息

| 名称    | 类型    | 是否必须 | 默认值 | 备注            | 其他信息                 |
| ------- | ------ | -------- | ------ | -------------- | ------------------------ |
| content | string | 非必须    |        | 文本           |                         |

##### 示例

```json
{
	"l2_type": 1,
	"body": {
		"content": "文本消息"
	}
}
```

#### 2.视频消息

| 名称            | 类型      | 是否必须 | 默认值 | 备注                         | 其他信息 |
| --------------- | --------- | -------- | ------ | ---------------------------- | -------- |
| video_info      | object [] | 非必须   |        | 视频消息                     | [{},{}]  |
| ├─ video_url    | string    | 非必须   |        | 视频地址                     |          |
| ├─ video_size   | string   | 非必须   |        | 视频数据大小，单位：字节     |          |
| ├─ video_second | integer   | 非必须   |        | 视频时长，单位：秒           |          |
| ├─ video_format | string    | 非必须   |        | 视频格式，例如 mp4           |          |
| ├─ thumb_url    | string    | 非必须   |        | 视频缩略图地址               |          |
| ├─ thumb_size   | string   | 非必须   |        | 缩略图大小，单位：字节       |          |
| ├─ thumb_width  | integer   | 非必须   |        | 缩略图宽度                   |          |
| ├─ thumb_height | integer   | 非必须   |        | 缩略图高度                   |          |
| ├─ thumb_format | string    | 非必须   |        | 缩略图格式，例如 JPG、BMP 等 |          |
| ├─ video_width  | integer   | 非必须   |        | 视频宽度                     |          |
| ├─ video_height | integer   | 非必须   |        | 视频高度                     |          |

##### 示例

```json
{
	"l2_type": 2,
	"body": {
		"video_info": [{
			"video_url": "https://www.example.com/video.mp4",
			"video_size": 54100,
			"video_second": 36,
			"video_format": "mp4",
			"thumb_url": "https://www.example.com/image.jpg",
			"thumb_width": 80,
			"thumb_height": 60,
			"thumb_format": "JPG",
			"video_width": 400,
			"video_height": 100
		}, {
			"video_url": "video.mp4",
			"video_size": 54100,
			"video_second": 36,
			"video_format": "mp4",
			"thumb_url": "https://www.example.com/image.jpg",
			"thumb_width": 80,
			"thumb_height": 60,
			"thumb_format": "JPG",
			"video_width": 400,
			"video_height": 100
		}]
	}
}
```

#### 3.图片消息

| 名称                | 类型      | 是否必须 | 默认值 | 备注                                 | 其他信息                                             |
| ------------------- | --------- | -------- | ------ | ------------------------------------ | ---------------------------------------------------- |
| pic_info            | object [] | 非必须   |        | 图片消息                             | [{},{}]                                              |
| ├─ uuid             | string    | 非必须   |        | 图片序列号。后台用于索引图片的键值。 |                                                      |
| ├─ image_format     | integer   | 非必须   | 0      | 图片格式                             | 枚举: JPG = 1，GIF = 2，PNG = 3，BMP = 4，其他 = 255 |
| ├─ image_info_array | object [] | 非必须   |        | 图片具体信息                         | [{},{}]                                              |
| ├─── type           | integer   | 非必须   |        | 图片类型                             | 1-原图，2-缩略图                                     |
| ├─── size           | integer   | 非必须   |        | 图片数据大小，单位：字节             |                                                      |
| ├─── width          | integer   | 非必须   |        | 图片宽度                             |                                                      |
| ├─── height         | integer   | 非必须   |        | 图片高度                             |                                                      |
| ├─── url            | string    | 非必须   |        | 图片地址                             |                                                      |
| ├───md5sum          | string    | 非必须   |        | 图片md5                              |                                                      |

##### 示例

```json
{
	"l2_type": 3,
	"body": {
		"pic_info": [{
				"uuid": "abc3788c-cf7c-447c-baae-30f38249ccc5",
				"image_format": 1,
				"image_info_array": [{
					"url": "https://www.example.com/image.jpg",
					"type": 1,
					"size": 4301,
					"width": 400,
					"height": 300,
					"md5sum": "03c7c0ace395d80182db07ae2c30f034"
				}]
			},
			{
				"uuid": "abc3788c-cf7c-447c-baae-30f38249ccc5",
				"image_format": 1,
				"image_info_array": [{
					"url": "https://www.example.com/image.jpg",
					"type": 2,
					"size": 4301,
					"width": 400,
					"height": 300,
					"md5sum": "03c7c0ace395d80182db07ae2c30f034"
				}]
			}
		]
	}
}
```

#### 4.文件消息

| 名称                | 类型        | 是否必须 | 默认值 | 备注                                 | 其他信息                                             |
| ------------------- | ---------  | -------- | ------ | ------------------------------------| ---------------------------------------------------- |
| file_info           | object[]   | 非必须   |         | 图片消息                             | [{},{}]                                              |
| ├── url             | string     | 非必须   |         |文件下载地址                          |                                                     |
| ├── file_size       | string    | 非必须   |         | 文件数据大小，单位：字节              |                                                     |
| ├── file_name       | string     | 非必须   |         |文件名称                             |                                                     |
| ├── download_flag   | integer    | 非必须   |         |文件下载方式标记                      |                                                     |


##### 示例

```json
{
	"l2_type": 4,
	"body": {
		"file_info": [
			{
				"url": "地址",
				"file_name": "文件名1",
				"file_size": 4301,
				"download_flag": 1
			},
			{
				"url": "地址",
				"file_name": "文件名2",
				"file_size": 4301,
				"download_flag": 1
			}
		]
	}
}
```

#### 5.音频消息

| 名称                | 类型        | 是否必须 | 默认值 | 备注                                 | 其他信息                                             |
| ------------------- | ---------  | ------- | ------ | ------------------------------------ | ---------------------------------------------------- |
| audio_info          | object[]   | 非必须   |        | 图片消息                             | [{},{}]                                              |
| ├── url             | string     | 非必须   |        |语音下载地址                           |                                                     |
| ├── size            | string    | 非必须   |        | 语音数据大小，单位：字节               |                                                     |
| ├── second          | string    | 非必须   |        |语音数据大小，单位：字节                |                                                     |
| ├── download_flag   | string    | 非必须   |        |文件下载方式标记                        |                                                     |


##### 示例

```json
{
	"l2_type": 5,
	"body": {
		"audio_info": [
			{
				"url": "地址",
				"second": 60,
				"size": 4301,
				"download_flag": 1
			},
			{
				"url": "地址",
				"second": 60,
				"size": 4301,
				"download_flag": 1
			}
		]
	}
}
```

#### 6.信令消息

| 名称                | 类型        | 是否必须 | 默认值 | 备注                                 | 其他信息                                             |
| ------------------- | ---------  | ------- | ------ | ------------------------------------ | ---------------------------------------------------- |
| signaling_msg       | object     | 非必须   |        | 信令消息(可忽略)                      | {}                                                   |
| ├── signaling_type  | integer    | 非必须   |        |信令消息类型                           |                                                     |
| ├── signaling_data  | string     | 非必须   |        | 信令消息数据                          |                                                     |



##### 示例

```json
{
	"l2_type": 6,
	"body": {
		"signaling_msg": 
			{
				"signaling_type": 1,
				"signaling_data": "sss",
			}
	}
}
```

#### 7.富文本消息

暂不提供(遇到这种类型的可先忽略)

#### 8.Markdown消息

| 名称    | 类型    | 是否必须 | 默认值 | 备注            | 其他信息                 |
| ------- | ------ | -------- | ------ | -------------- | ------------------------ |
| content | string | 非必须    |        | markdown       |                         |

##### 示例

```json
{
	"l2_type": 8,
	"body": {
		"content": "markdown"
	}
}
```

#### 9.卡片消息

| 名称                | 类型        | 是否必须 | 默认值 | 备注                                 | 其他信息                                             |
| ------------------- | ---------  | ------- | ------ | ------------------------------------ | ---------------------------------------------------- |
| card_info           | object []  | 非必须  |        | 卡片消息                              | [{},{}]                                            |
| ├── link            | string     | 非必须  |        | 链接                                  |                                                     |
| ├── thumbnail       | string     | 非必须  |        | 缩略图                                |                                                     |
| ├── title           | string     | 非必须  |        | 标题                                  |                                                     |
| ├── source          | string     | 非必须  |        | 来源                                  |                                                     |
| ├── ext             | object[]   | 非必须  |        | 扩展字段                               | [{},{}]                                             |
| ├──── key           | string     | 非必须  |        | key                                   |                                                     |
| ├──── value         | string     | 非必须  |        | value                                 |                                                     |


##### 示例

```json
{
	"l2_type": 9,
	"body": {
		"card_info": [
			{
				"link": "https://www.example.com/image.jpg",
				"thumbnail": "https://www.example.com/image.jpg",
				"title":"标题",
				"source":"来源",
				"ext":[
					{"key":"width","value":"111"}
				]
			},
			{
				"link": "https://www.example.com/image.jpg",
				"thumbnail": "https://www.example.com/image.jpg",
				"title":"标题",
				"source":"来源",
				"ext":[
					{"key":"width","value":"111"}
				]
			}
		]
	}
}
```

#### 10.系统消息

暂不提供(遇到这种类型的可先忽略)

#### 11.表情消息

| 名称                    | 类型        | 是否必须 | 默认值  | 备注                                 | 其他信息                                             |
| -------------------     | ---------  | -------  | ------ | ------------------------------------ | ---------------------------------------------------- |
| sticker_msg             | object     | 非必须    |        | 表情消息                             |                                                     |
| ├── sticker_id          | string    | 非必须    |        | 表情id                               |                                                     |
| ├── sticker_package_id  | string    | 非必须    |        | 表情包id                             |                                                     |
| ├── url                 | string     | 非必须    |        | 表情包图片链接                        |                                                     |
| ├── width               | integer    | 非必须    |        | 图片宽度                              |                                                     |
| ├── height              | integer    | 非必须    |        | 图片高度                              |                                                     |
| ├── md5sum              | string     | 非必须    |        | 表情图md5                             |                                                     |


##### 示例

```json
{
	"l2_type": 11,
	"body": {
		"sticker_msg": 
			{
				"sticker_id":1,
				"sticker_package_id": 12,
				"url":"地址",
				"width":125,
				"height":125,
				"md5sum":""
			}
	}
}
```

#### 12.混合消息

| 名称             | 类型      | 是否必须 | 默认值 | 备注             | 其他信息                      |
| ---------------- | --------- | -------- | ------ | ---------------- | ----------------------------- |
| mixed_msg        | object    | 非必须   |        | 混合消息排序控制 | 文本、图片、视频 排序         |
| ├─ msg_item_list | object [] | 非必须   |        | 排序规则         | [{"l2_type":1},{"l2_type":2}] |
| ├─── l2_type     | integer   | 非必须   |        | 待排序的消息类型 |                               |


##### 示例

```json
{
	"l2_type": 12,
	"body": {
		"content": "文本+图片混合消息",
		"pic_info": [{
			"uuid": "abc3788c-cf7c-447c-baae-30f38249ccc5",
			"image_format": 1,
			"image_info_array": [{
				"url": "https://www.example.com/image.jpg",
				"type": 2,
				"size": 4301,
				"width": 400,
				"height": 300,
				"md5sum": "03c7c0ace395d80182db07ae2c30f034"
			}]
		}],
		"mixed_msg": {
			"msg_item_list": [{
					"l2_type": 3
				},
				{
					"l2_type": 1
				}
			]
		}
	}

}
```

#### 13.互动类消息

| 名称                       | 类型        | 是否必须 | 默认值 | 备注                                    | 其他信息                      |
| ----------------           | ---------  | -------- | ------ | ----------------                       | ----------------------------- |
| interaction_msg            | object[]   | 非必须   |        | 互动类消息（可忽略类型）                  | [{},{}]                       |
| ├── choose_type            | integer    | 非必须   |        | 选择类型 1:单选 2:多选                   |                               |
| ├── enable_cancel          | integer    | 非必须   |        | 是否支持取消 1:是,0:否                    |                               |
| ├── interactions           | object[]   | 非必须   |        | 单项明细                                 |  [{},{}]                             |
| ├───── id                  | string    | 非必须   |        | id                                      |                               |
| ├───── content             | string     | 非必须   |        | 文字+表情                                |                               |
| ├───── statistics          | string    | 非必须   |        | 统计数量                                 |                               |
| ├───── type                | integer    | 非必须   |        | 类型,1:指令,2:投票,3:接口                 |                               |
| ├───── if_show_result      | integer    | 非必须   |        | 否显示统计结果,1:是,0:否                  |                               |
| ├───── if_clicked          | integer    | 非必须   |        | 是否已经点过,1:是,0:否                    |                               |
| ├───── interaction_id      | string    | 非必须   |        | id                                       |                               |


##### 示例

```json
{
	"l2_type": 13,
	"body": {
		"interaction_msg": {
			"choose_type": 1,
			"enable_cancel":1,
			"interactions": [{
				"id": 1,
				"content": "A",
				"statistics":1,
				"type": 1,
				"if_show_result":1,
				"if_clicked":1,
				"interaction_id":1
			},
			{
				"id": 2,
				"content": "B",
				"statistics":1,
				"type": 1,
				"if_show_result":1,
				"if_clicked":1,
				"interaction_id":1
			}]
		}
	}

}
```

#### 机器人消息

| 名称      | 类型    | 是否必须 | 默认值 | 备注                | 其他信息 |
| --------- | ------- | -------- | ------ | ------------------- | -------- |
| bot_data  | object  | 非必须   |        | Bot特殊消息         |          |
| ├─ cmd_id | string | 非必须   |        | Bot消息命中的指令ID |          |

##### 示例

```json
	{
		"body": {
			"bot_data":{
				"cmd_id":10000086
			}
	}
```

### l3_type说明

#### 1.回复消息

| 名称           | 类型    | 是否必须 | 默认值 | 备注                    | 其他信息 |
| -------------- | ------- | -------- | ------ | ----------------------- | -------- |
| reply_msg      | object  | 非必须   |        | 回复消息                | {}       |
| ├─ uid_replied | string | 非必须   |        | 被回复者uid             |          |
| ├─ content     | string  | 非必须   |        | 被回复的消息内容,展示用 |          |
| ├─ msg_seq     | string | 非必须   |        | 被回复的消息唯一标识    |          |
| ├─ msg_id      | string  | 非必须   |        | 消息id                  |          |

##### 示例

```json
{
	"l3_types": [1],
	"body": {
		"reply_msg": {
			"content": "[图片]",
			"uid_replied": "10000086",
			"msg_seq": "200002121210000086",
			"msg_id": "03c7c0ace395d80182db07ae2c30f034"
		}
	}
}
```

#### 3.@消息

| 名称           | 类型       | 是否必须 | 默认值 | 备注                                 | 其他信息          |
| -------------- | ---------- | -------- | ------ | ------------------------------------ | ----------------- |
| at_msg         | object     | 非必须   |        | @消息 at消息                         | {}                |
| ├─ at_type     | integer    | 非必须   | 0      | @类型,部分@时候才需要填写at_uid_list | 1-部分人 2-所有人 |
| ├─ at_uid_list | string [] | 非必须   |        | 被@用户id列表                        |                   |

##### 示例

```json
{
	"l3_types": [3],
	"body": {
		"at_msg": {
			"at_type": 1,
			"at_uid_list": [10000086, 100000032]
		}
	}
}
```


#### 4.链接消息

| 名称            | 类型      | 是否必须 | 默认值 | 备注                | 其他信息                    |
| --------------- | --------- | -------- | ------ | ------------------- | --------------------------- |
| link_to_msg     | object [] | 非必须   |        | 链接跳转消息        | [{},{}]                     |
| ├─ type         | integer   | 非必须   | 0      | 链接类型            | 1-频道；2-外站；3-Bot设置页 |
| ├─ display_name | string    | 非必须   |        | 显示文案            |                             |
| ├─ channel_ext  | object    | 非必须   |        | 频道跳转内容 type=1 | {}                          |
| ├─── gid        | string   | 非必须   |        | 群组ID              | 仅支持相同群组频道跳转      |
| ├─── cid        | string    | 非必须   |        | 频道ID              |                             |
| ├─ website_ext  | object    | 非必须   |        | 外站跳转内容 type=2 | {}                          |
| ├─── url        | string    | 非必须   |        | 链接地址            | 必须 https                  |
| ├─ botconf_ext  | object    | 非必须   |        | Bot设置页 type=3    | {}                          |
| ├─── gid        | string    | 非必须   |        | 群组ID              | 仅支持相同群组Bot配置页跳转 |
| ├─── bot_id     | string    | 非必须   |        | Bot ID              |                             |

##### 示例

```json
{
	"l3_types": [4],
	"body": {
		"link_to_msg": [{
			"type": 3,
			"displayName": " 配置跳转 ",
			"botconf_ext": {
				"gid": 10086,
				"bot_id": 100000001
			}
		}, {
			"type": 1,
			"displayName": "频道跳转 ",
			"channel_ext": {
				"gid": 10086,
				"cid": "10088"
			}
		}, {
			"type": 2,
			"displayName": "外站跳转 ",
			"website_ext": {
				"url": "https://www.example.com/"
			}
		}]
	}
}
```

### 完整消息示例

```json
     {
     	"signal": 1,
     	"data": [{
     		"type": "common",
     		"scope": "channel",
     		"l2_type": 8,
     		"sender_uid": 100000030,
     		"msg_id": "2_18909_1668",
     		"gid": 15535,
     		"target_id": "18909",
     		"ts": 1623292203,
     		"body": {
     			"content": "微博今日热门精选！\n\r[#一周电视剧报告# 2021年05月24日-05月30日 权威发布 祝贺🎉《月光变奏曲》《乌鸦小姐与蜥蜴先生》《御赐小仵作》...](https://overseas.weibo.com/5406006781/KjuYQulhT)\n\r[#妈妈回应被女儿抱住后放弃气球#【#母亲回应被女儿抱住瞬间气球刮飞#[抱一抱]】近日，山东淄博，一对母女卖气球突...](https://overseas.weibo.com/2810373291/KjuPCbIhp)\n\r[【heraldpop】[《EXO娱乐馆第二季》今天（10日）公开最后一集]《给你看EXO ：EXO娱乐馆第二季》是通过Youtube EXO...](https://overseas.weibo.com/2482557597/Kjv08xTFo)\n\r[【iMBC】[NCT DREAM《Hot Sauce》荣登5月Gaon排行榜1位今日公开Remix版本]5月10日发售的NCT DREAM首张正规专辑《H...](https://overseas.weibo.com/2482557597/KjuY70cQF)\n\r[转发@电影盛夏未来：#盛夏未来新预告# 这是一段关于盛夏的快乐宣言， 这是一段对于未来的勇往直前， 这就是，属于...](https://overseas.weibo.com/2142058927/KjuUmvhMv)\n\r[#星星调查# 帮我家猪做做伸展瑜伽🧘 它很配合 并且一脸开心😸#我的宠物最丑那张照片# ​](https://overseas.weibo.com/2044833997/Kjv0vCZ8i)\n\r[转发@电影盛夏未来：#盛夏未来新预告# 这是一段关于盛夏的快乐宣言， 这是一段对于未来的勇往直前， 这就是，属于...](https://overseas.weibo.com/1353112775/KjuOjwv7X)\n\r",
     			"bot_data": {
     				"cmd_id": 4
     			}
     		},
     		"nonce": "5WEJ5cp4a0"
     	}]
     }
```
