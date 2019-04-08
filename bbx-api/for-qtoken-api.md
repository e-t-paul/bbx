# qtoken定制版接口文档  
本文档用于Qtoken dapp 接口对接

- [1.当前预测期数信息](#当前预测期数信息)
- [2.预测](#预测)
- [3.最后预测结果](#最后预测结果)
- [4.往期预测记录](#往期预测记录)
- [5.百宝箱统计](#百宝箱统计)
- [6.通知](#通知)
- [7.我的预测](#我的预测)


## 当前预测期数信息
### GET  /v1/bet/currentPhraseDetail

**实现功能：**获取当前可投注期数信息的详细信息

请求参数

| 参数名称 | 是否必须 | 数据类型 | 描述                        | 取值范围 |
| -------- | :------: | -------- | --------------------------- | -------- |
| type     |   true   | int      | 由1—4分别代表对应的几维空间 | 1—4      |

响应数据

|  参数名称   | 是否必须 | 数据类型 | 描述                                                         | 取值范围 |
| :---------: | -------- | -------- | ------------------------------------------------------------ | -------- |
|   phrase    | true     | int      | 当前期数                                                     |          |
|    pool     | true     | float64  | 当前奖池金额                                                 |          |
|    price    | true     | float64  | 每注单价                                                     |          |
|    item     | true     | object   | 当前下注统计。   eg:type=1,分别表示出数字和字母的下注金额。   type=2，依次表示下注0-f的下注金额。   type=3，依次表示下注0、1、2、3、4、5的下注金额。   type=4,表示全猜中。 |    null or object      |
| bettingitem | true     | object   | 每一个投注项  被投的注数                                     |  null or object        |
| playeritem  | true     | object   | 每一个投注项  被投的次数                                     |  null or object        |

请求响应示例
```
/* 1. 获取一维空间可投注期数信息的详情
/* GET /v1/bet/currentPhraseDetail?type=1 
 */ 
{
	"status": "ok",
	"data": {
		"phrase": 21451,
		"pool": 42,
		"price": 1,
		"item": {
			"0": 30,
			"1": 12
		},
		"betting_item": {
			"0": 30,
			"1": 12
		},
		"player_item": {
			"0": 12,
			"1": 3
		}
	}
}


/* 1.2 获取一维空间可投注期数信息的详情,没有人参与时的数据,二三四维空间类似，不再累赘
/* GET /v1/bet/currentPhraseDetail?type=1 
 */ 
{
	"status": "ok",
	"data": {
		"phrase": 570119,
		"pool": 0,
		"price": 1,
		"item": null,
		"betting_item": null,
		"player_item": null
	}
}


/* 2. 获取二维空间可投注期数信息的详情
/* GET /v1/bet/currentPhraseDetail?type=2
 */ 
{
	"status": "ok",
	"data": {
		"phrase": 21451,
		"pool": 106,
		"price": 20,
		"item": {
			"0": 3,
			"1": 0,
			"2": 0,
			"3": 0,
			"4": 100,
			"5": 0,
			"6": 0,
			"7": 0,
			"8": 0,
			"9": 0,
			"a": 0,
			"b": 0,
			"c": 0,
			"d": 0,
			"e": 0,
			"f": 3
		},
		"betting_item": {
			"0": 3,
			"1": 0,
			"2": 0,
			"3": 0,
			"4": 5,
			"5": 0,
			"6": 0,
			"7": 0,
			"8": 0,
			"9": 0,
			"a": 0,
			"b": 0,
			"c": 0,
			"d": 0,
			"e": 0,
			"f": 3
		},
		"player_item": {
			"0": 3,
			"1": 0,
			"2": 0,
			"3": 0,
			"4": 2,
			"5": 0,
			"6": 0,
			"7": 0,
			"8": 0,
			"9": 0,
			"a": 0,
			"b": 0,
			"c": 0,
			"d": 0,
			"e": 0,
			"f": 1
		}
	}
}


/* 3. 获取三维空间可投注期数信息的详情
/* GET /v1/bet/currentPhraseDetail?type=3
 */  
{
	"status": "ok",
	"data": {
		"phrase": 21451,
		"pool": 11,
		"price": 1,
		"item": {
			"0": 1,
			"1": 2,
			"2": 3,
			"3": 2,
			"4": 1,
			"5": 2
		},
		"betting_item": {
			"0": 1,
			"1": 2,
			"2": 3,
			"3": 2,
			"4": 1,
			"5": 2
		},
		"player_item": {
			"0": 1,
			"1": 2,
			"2": 3,
			"3": 2,
			"4": 1,
			"5": 1
		}
	}
}


/* 4. 获取四维空间可投注期数信息的详情
/* GET /v1/bet/currentPhraseDetail?type=4
 */  
{
	"status": "ok",
	"data": {
		"phrase": 21456,
		"pool": 125.25,
		"price": 1,
		"item": {
			"12345": 11,
			"65432": 11
		},
		"betting_item": {
			"12345": 11,
			"65432": 11
		},
		"player_item": {
			"12345": 1,
			"65432": 1
		}
	}
}


/* 5. 请求失败返回
/* GET /v1/bet/currentPhraseDetail?type=0
 */
{
	"status": "error",
	"err-msg": "<QuerySeter> no row found"
}
```

## 预测
### GET /v1/bet/bet2

**实现功能：**Qtoken预测接口，服务端验证预测信息和付款凭证。

请求参数

| 参数名称 | 是否必须 | 数据类型 | 描述                                | 取值范围               |
| -------- | -------- | -------- | ----------------------------------- | ---------------------- |
| type     | true     | int      | 游戏类别。1—4分别代表对应的几维空间 | 1--4                   |
| phrase   | true     | int      | 竞猜期数                            | “必须是当前可投注期数” |
| choice   | true     | int      | 投注项                              |                        |
| qty      | true     | int      | 注数                                |                        |
| voucher  | true     | string   | 付款凭证                            |                        |

​

响应数据

| 参数名称 | 是否必须 | 数据类型 | 描述               | 取值范围 |
| -------- | -------- | -------- | ------------------ | -------- |
| id       | true     | int      | 竞猜记录id         |          |
| time     | true     | int64    | 这条记录创建的时间 |          |

请求响应示例

```
/* 1. 获取一维空间完成投注的结果 二维，三维，四维返回数据相同
/* GET /v1/bet/bet2?type=1&phrase=570146&choice=0&qty=1&voucher=wangleitest
 */
{
	"status": "ok",
	"data": {
		"id": 227,
		"time": 1554366498
	}
}

/* 2. 请求失败返回
/* GET /v1/bet/bet2?type=1&phrase=570146&choice=0&qty=1&voucher=hahah
 */
{
	"status": "error",
	"err-msg": "验签失败，暂未实现"
}
```




## 最后预测结果
### GET /v1/bet/lastPhraseDetail

**实现功能：**获取最近一次游戏的类型数据

请求参数

| 参数名称 | 是否必须 | 数据类型 | 描述                        | 取值范围 |
| -------- | -------- | -------- | --------------------------- | -------- |
| type     | true     | int      | 由1—4分别代表对应的几维空间 | 1—4      |

响应数据

| 参数名称      | 是否必须 | 数据类型 | 描述               | 取值范围 |
| ------------- | -------- | -------- | ------------------ | -------- |
| phrase        | true     | int      | 竞猜期数           |          |
| produceresult | true     | string   | 下注人自己选的结果 |          |
| winner        | true     | int      | 中奖人数           |          |
| pool          | true     | float64  | 当前奖池金额       |          |

请求响应示例

``` 
/* 1. 获取一维空间最近一次游戏的类型数据
/* GET "  /v1/bet/lastPhraseDetail?type=1"
 */
{
	"status": "ok",
	"data": {
		"phrase": 21440,
		"result": "1",
		"winner": 0,
		"pool": 0
	}
}


/* 2. 获取二维空间最近一次游戏的类型数据
/* GET "  /v1/bet/lastPhraseDetail?type=2"
 */
{
	"status": "ok",
	"data": {
		"phrase": 21440,
		"result": "c",
		"winner": 0,
		"pool": 0
	}
}


/* 3. 获取三维空间最近一次游戏的类型数据
/* GET "  /v1/bet/lastPhraseDetail?type=3"
 */
{
	"status": "ok",
	"data": {
		"phrase": 21440,
		"result": "0",
		"winner": 0,
		"pool": 0
	}
}


/* 4. 获取四维空间最近一次游戏的类型数据
/* GET "  /v1/bet/lastPhraseDetail?type=4"
 */
{
	"status": "ok",
	"data": {
		"phrase": 21312,
		"result": "68f95",
		"winner": 0,
		"pool": 0
	}
}


/* 5. 请求失败返回
/* GET "  /v1/bet/lastPhraseDetail?type=0"
 */
{
	"status": "error",
	"err-msg": "获取失败"
}
```

## 往期预测记录

### GET /v1/bet/history

**实现功能：**获取已开奖场次的开奖信息

请求参数

| 参数名称 | 是否必须 | 数据类型 | 描述                                | 取值范围           |
| -------- | -------- | -------- | ----------------------------------- | ------------------ |
| type     | true     | int      | 游戏类别。1—4分别代表对应的几维空间 | 1--4               |
| sortby   | false    | string   | 排序列                              | 'Phrase' default:''     |
| order    | false    | string   | 排序方式：                          | 'desc' or 'asc' default:asc |
| page     | false    | int      | 页数                            | 默认 1             |
| limit    | false    | int      | 每页记录数                            | 默认10             |

响应数据

| 参数名称  | 是否必须 | 数据类型 | 描述                   | 取值范围 |
| --------- | -------- | -------- | ---------------------- | -------- |
| id        | true     | int      | 竞猜记录id             |          |
| phrase    | true     | int      | 竞猜期数               |          |
| status    | true     | string   | 开奖状态               |          |
| result    | true     | string   | 开奖结果               |          |
| guessnum  | true     | int      | 停止投注时竞猜参与人数 |          |
| winner    | true     | int      | 中奖人个数             |          |
| pool      | true     | float64  | 当前奖池金额           |          |
| blockhash | true     | string   | 区块哈希               |          |

请求响应示例

``` 
/* 1. 获取一维空间已开奖场次的开奖信息
/* GET  "/v1/bet/history?type=1"
 */
{
	"code": 0,
	"count": 491,
	"data": [{
			"Id": 23378,
			"Phrase": 20961,
			"Status": "已开奖",
			"Result": "数字(流局)",
			"GuessNum": 0,
			"Winner": 0,
			"Pool": 0,
			"BlockHash": "223bc7cf4b837c6c993d24fd6f29f7b73c006e1710a810b9f267424c5abf4ee1"
		},
		{
			"Id": 23382,
			"Phrase": 20962,
			"Status": "已开奖",
			"Result": "数字(流局)",
			"GuessNum": 0,
			"Winner": 0,
			"Pool": 0,
			"BlockHash": "72142a280ff0219cc322e8259a037fc7dea43df156f36ac393fe21c1c60b8210"
		},
		{
			"Id": 23385,
			"Phrase": 20963,
			"Status": "已开奖",
			"Result": "数字",
			"GuessNum": 2,
			"Winner": 1,
			"Pool": 10001,
			"BlockHash": "36bb814342327bb596e8687f43c7feaabcbff69d49c18662508d368534a82e69"
		},
		{
			"Id": 23388,
			"Phrase": 20964,
			"Status": "已开奖",
			"Result": "数字(流局)",
			"GuessNum": 0,
			"Winner": 0,
			"Pool": 0,
			"BlockHash": "68b2b1700e094460361c3787d21125db86cc12b4e104d3511095f1ba3187fd98"
		},
		{
			"Id": 23391,
			"Phrase": 20965,
			"Status": "已开奖",
			"Result": "字母(流局)",
			"GuessNum": 0,
			"Winner": 0,
			"Pool": 0,
			"BlockHash": "5d09a318253f38094f9e8fb6e31e48ba0694d7914a28a6329f1bbe3cc1da27bb"
		},
		{
			"Id": 23394,
			"Phrase": 20966,
			"Status": "已开奖",
			"Result": "数字(流局)",
			"GuessNum": 0,
			"Winner": 0,
			"Pool": 0,
			"BlockHash": "2468c7bd80dee51ee2a8de4abbc17e5127c5294af0ded4c251068a62e7b340f7"
		},
		{
			"Id": 23397,
			"Phrase": 20967,
			"Status": "已开奖",
			"Result": "数字(流局)",
			"GuessNum": 0,
			"Winner": 0,
			"Pool": 0,
			"BlockHash": "055300400193ae15b6f96ca2215c2f9ecf5c4102b64c12c1c7dd372a723f65f2"
		},
		{
			"Id": 23400,
			"Phrase": 20968,
			"Status": "已开奖",
			"Result": "数字(流局)",
			"GuessNum": 0,
			"Winner": 0,
			"Pool": 0,
			"BlockHash": "064106e9e2fc88cecb5cac0bc2f00a1f7cfe90443fb1eff69797ae2c33089fa3"
		},
		{
			"Id": 23403,
			"Phrase": 20969,
			"Status": "已开奖",
			"Result": "数字(流局)",
			"GuessNum": 0,
			"Winner": 0,
			"Pool": 0,
			"BlockHash": "66239dec50a8bed25d6d3e43741c35558c52badbdad73f3116aa1960c4b0dcc8"
		},
		{
			"Id": 23406,
			"Phrase": 20970,
			"Status": "已开奖",
			"Result": "数字(流局)",
			"GuessNum": 0,
			"Winner": 0,
			"Pool": 0,
			"BlockHash": "1a7cf97333d59c7147ea2b9b38da28fb411805c6151e559e3a6815ac1518d016"
		}
	],
	"msg": "success"
}

/* 2. 获取一维空间往期预测记录，按期数逆序排序，获取第一页2条记录
/* GET  /v1/bet/history?type=1&page=1&limit=2&sortby=Phrase&order=desc
 */
{
	"status": "ok",
	"data": {
		"total": 786,
		"count": 2,
		"list": [{
				"Id": 25741,
				"Phrase": 570697,
				"Status": "进行中",
				"Result": "",
				"GuessNum": 0,
				"Winner": 0,
				"Pool": 0,
				"BlockHash": ""
			},
			{
				"Id": 25738,
				"Phrase": 570696,
				"Status": "未开奖",
				"Result": "",
				"GuessNum": 0,
				"Winner": 0,
				"Pool": 0,
				"BlockHash": ""
			}
		]
	}
}


/*  3. 请求失败返回
/*  GET "/v1/bet/history?type=0"
 */
{
	"code": 501,
	"count": 0,
	"data": null,
	"msg": "获取失败"
}
```

## 百宝箱统计

### GET /v1/bet/stat

**实现功能：**获取投注统计数据

请求参数

无

响应数据

| 参数名称 | 是否必须 | 数据类型 | 描述             | 取值范围 |
| -------- | -------- | -------- | ---------------- | -------- |
| count    | true     | int      | 当前累计游戏人次 |          |
| amount   | true     | float64  | 当前奖池金额     |          |

请求响应示例

``` 
/* 1. 获取投注统计数据
/* GET  /v1/bet/stat
 */
{
	"status": "ok",
	"data": {
		"count": 68,
		"amount": 10883
	}
}

/* 2. 请求失败返回
/* GET  /v1/bet/stat
 */
{
	"status": "error",
	"err-msg": "获取失败"
}
```

## 通知 

### GET /v1/public/announce

**实现功能：**从服务端拉取通告，通知，活动等信息

请求参数

| 参数名称 | 是否必须 | 数据类型 | 描述                                                   | 取值范围 |
| -------- | -------- | -------- | ------------------------------------------------------ | -------- |
| type     | int      | true     | 类别0：所有类型。1：系统通知。2：表示公告。3：表示活动 | 0--3     |
| page   | int      | false    | 页数                                               |     默认1     |
| limit    | int      | false    | 每页记录数                                               |   默认10       |

响应数据

| 参数名称 | 是否必须 | 数据类型 | 描述     | 取值范围 |
| -------- | -------- | -------- | -------- | -------- |
| title    | true     | string   | 活动标题 |          |
| time     | true     | int      | 发布时间 |          |
| url      | true     | string   | 详情页面 |          |

请求响应示例

``` 
/* 1. 从服务端拉取所有类型信息
/* GET /v1/public/announce?type=0&offset=0&limit=2
 */
{
	"status": "ok",
	"data": {
		"total": 2,
		"count": 2,
		"list": [{
				"title": "第一条测试公告",
				"time": 1552038018,
				"url": "http://baidu.com"
			},
			{
				"title": "第二条测试公告",
				"time": 1552038150,
				"url": "http://taobao.com"
			}
		]
	}
}


/* 2. 请求失败返回
/* GET /v1/public/announce?type=4&offset=0&limit=2
 */
{
	"status": "error",
	"err-msg": "获取公告通知活动失败"
}
```

## 我的预测

### GET /v1/personal/bet

**实现功能：**获取个人竞猜投票列表

请求参数

| 参数名称   | 是否必须 | 数据类型 | 描述                           | 取值范围 |
| ---------- | -------- | -------- | ------------------------------ | -------- |
| page       | false    | int      | 页数                       | 默认1    |
| limit      | false    | int      | 每页记录数                       | 默认10   |
| starttoend | false    | string   | 起始--截止时间                 |          |
| type       | false    | int      | 空间名称（1--4对应的维度空间） |          |

响应数据

| 参数名称    | 是否必须 | 数据类型 | 描述     |                           取值范围                           |
| ----------- | -------- | :------: | -------- | :----------------------------------------------------------: |
| Id          | true     |   int    | 记录id   |                                                              |
| SpaceName   | true     |  string  | 玩法名称 | 'One-dimensional';'Two-dimensional';'Three-dimensional','Four-dimensional'   ('一维空间','二维空间','三维空间','四维空间') |
| BlockHeight | true     |   int    | 当前期数 |                                                              |
| Choice      | true     |  string  | 投注项   |                                                              |
| Qty         | true     |   int    | 注数     |                                                              |
| BonusStatus | true     |   int    | 开奖状态 |                 "开奖状态0 未中奖 1 已中奖"                  |
| GuessStatus | true     |   int    | 结算状态 |                      "0未结算，1已结算"                      |
| Bonus       | true     | float64  | 题目红利 |                                                              |

请求响应示例

``` 
/* 1. 获取个人竞猜投票列表
/* GET /v1/personal/bet?page=1&limit=10 
 */
{
  "status": "ok",
  "data": {
    "total": 1,
    "count": 1,
    "list": [
      {
        "Id": 227,
        "SpaceName": "一维空间",
        "BlockHeight": 570146,
        "Choice": "0",
        "Qty": 1,
        "BonusStatus": 0,
        "GuessStatus": 1,
        "Bonus": 0
      }
    ]
  }
}


/* 2. 请求失败返回
/* GET /v1/personal/bet?offset=0&limit=2
 */
{
	"status": "error",
	"err-msg": "获取失败"
}
```


