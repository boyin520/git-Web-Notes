# 开放通用Api，总有你喜欢的

# **前言**

前段时间做了一个小插件，需要调用一个查询指定期号中奖号码的Api接口，找了很多开放的接口，都不合我意，要么限速，要么收费，要么进群。还可能不稳定，接口动不动就被改掉了，导致访问失败。遂罢。

自己撸一个！

首先实现的是给自己用的福彩相关的Api，推荐给朋友后试着还不错，于是在朋友的推荐下新增了一些其他的api接口，为之购买了独立的服务器并部署了项目，目前域名正在备案中。

# **目标**

我会坚持维护，也会积极响应朋友的号召，有好的意见也会积极采纳并实施。更重要的是：接口**不限速**，**不收费**，**不加群**，但希望你不要频繁请求，注意优化自己的逻辑，频繁调用系统可能会禁用你的ip，导致你无法正常请求。更不要恶意攻击，且行且珍惜。

# **说明**

**博客中的Api文档是截止昨晚发布的，不会实时更新**，以后新增的Api接口以及详细的文档说明都会在Github上进行，我会尽心维护，尽力写好文档。当然在使用过程中有什么问题或者建议，最好是在Github的issue中提出来或者直接联系我。您的star就是对我最大的鼓励！

GIthub地址：**github.com/MZCretin/Ro…**

常用邮箱：**mxnzp_life@163.com**

常用QQ：**792075058**

个人站点主页：**www.mxnzp.com**

# **接口文档**

## 目录

- [通用](https://juejin.im/post/5c062f0ce51d451d992a973a#%E9%80%9A%E7%94%A8)
- [更新记录](https://juejin.im/post/5c062f0ce51d451d992a973a#%E6%9B%B4%E6%96%B0%E8%AE%B0%E5%BD%95)
- 接口列表
  - 一、福彩-双色球接口
    - [**指定期号中奖号码**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E6%8C%87%E5%AE%9A%E6%9C%9F%E5%8F%B7%E4%B8%AD%E5%A5%96%E5%8F%B7%E7%A0%81)
    - [**最新中奖号码信息**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E6%9C%80%E6%96%B0%E4%B8%AD%E5%A5%96%E5%8F%B7%E7%A0%81%E4%BF%A1%E6%81%AF)
    - [**获取双色球中奖信息列表**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E8%8E%B7%E5%8F%96%E5%8F%8C%E8%89%B2%E7%90%83%E4%B8%AD%E5%A5%96%E4%BF%A1%E6%81%AF%E5%88%97%E8%A1%A8)
  - 二、节假日及万年历
    - [**指定日期的节假日及万年历信息**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E6%8C%87%E5%AE%9A%E6%97%A5%E6%9C%9F%E7%9A%84%E8%8A%82%E5%81%87%E6%97%A5%E5%8F%8A%E4%B8%87%E5%B9%B4%E5%8E%86%E4%BF%A1%E6%81%AF)
    - [**指定多个日期的节假日及万年历信息**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E6%8C%87%E5%AE%9A%E5%A4%9A%E4%B8%AA%E6%97%A5%E6%9C%9F%E7%9A%84%E8%8A%82%E5%81%87%E6%97%A5%E5%8F%8A%E4%B8%87%E5%B9%B4%E5%8E%86%E4%BF%A1%E6%81%AF)
    - [**指定月份所有的节假日及万年历信息**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E6%8C%87%E5%AE%9A%E6%9C%88%E4%BB%BD%E6%89%80%E6%9C%89%E7%9A%84%E8%8A%82%E5%81%87%E6%97%A5%E5%8F%8A%E4%B8%87%E5%B9%B4%E5%8E%86%E4%BF%A1%E6%81%AF)
    - [**指定月份指定类型的所有的节假日及万年历信息**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E6%8C%87%E5%AE%9A%E6%9C%88%E4%BB%BD%E6%8C%87%E5%AE%9A%E7%B1%BB%E5%9E%8B%E7%9A%84%E6%89%80%E6%9C%89%E7%9A%84%E8%8A%82%E5%81%87%E6%97%A5%E5%8F%8A%E4%B8%87%E5%B9%B4%E5%8E%86%E4%BF%A1%E6%81%AF)
    - [**指定年份所有的节假日及万年历信息**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E6%8C%87%E5%AE%9A%E5%B9%B4%E4%BB%BD%E6%89%80%E6%9C%89%E7%9A%84%E8%8A%82%E5%81%87%E6%97%A5%E5%8F%8A%E4%B8%87%E5%B9%B4%E5%8E%86%E4%BF%A1%E6%81%AF)
    - [**指定年份指定类型的所有的节假日及万年历信息**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E6%8C%87%E5%AE%9A%E5%B9%B4%E4%BB%BD%E6%8C%87%E5%AE%9A%E7%B1%BB%E5%9E%8B%E7%9A%84%E6%89%80%E6%9C%89%E7%9A%84%E8%8A%82%E5%81%87%E6%97%A5%E5%8F%8A%E4%B8%87%E5%B9%B4%E5%8E%86%E4%BF%A1%E6%81%AF)
  - 三、全国城市列表（全国地级市API，数据来源国家统计局）
    - [**全国城市列表**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E5%85%A8%E5%9B%BD%E5%9F%8E%E5%B8%82%E5%88%97%E8%A1%A8)
    - [**搜索全国城市列表**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E6%90%9C%E7%B4%A2%E5%85%A8%E5%9B%BD%E5%9F%8E%E5%B8%82%E5%88%97%E8%A1%A8)
  - 四、IP地址信息
    - [**获取访问者的ip地址信息**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E8%8E%B7%E5%8F%96%E8%AE%BF%E9%97%AE%E8%80%85%E7%9A%84ip%E5%9C%B0%E5%9D%80%E4%BF%A1%E6%81%AF)
    - [**获取指定ip的ip地址信息**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E8%8E%B7%E5%8F%96%E6%8C%87%E5%AE%9Aip%E7%9A%84ip%E5%9C%B0%E5%9D%80%E4%BF%A1%E6%81%AF)
  - 五、小工具
    - [**获取不重复长ID**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E8%8E%B7%E5%8F%96%E4%B8%8D%E9%87%8D%E5%A4%8D%E9%95%BFid)
    - [**获取不重复短ID**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E8%8E%B7%E5%8F%96%E4%B8%8D%E9%87%8D%E5%A4%8D%E7%9F%ADid)
  - 六、天气信息
    - [**获取特定城市今日天气**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E8%8E%B7%E5%8F%96%E7%89%B9%E5%AE%9A%E5%9F%8E%E5%B8%82%E4%BB%8A%E6%97%A5%E5%A4%A9%E6%B0%94)
    - [**获取特定城市今天及未来天气**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E8%8E%B7%E5%8F%96%E7%89%B9%E5%AE%9A%E5%9F%8E%E5%B8%82%E4%BB%8A%E5%A4%A9%E5%8F%8A%E6%9C%AA%E6%9D%A5%E5%A4%A9%E6%B0%94)
  - 七、笑话段子
    - [**分页获取笑话段子列表**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E5%88%86%E9%A1%B5%E8%8E%B7%E5%8F%96%E7%AC%91%E8%AF%9D%E6%AE%B5%E5%AD%90%E5%88%97%E8%A1%A8)
    - [**随机获取笑话段子列表**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E9%9A%8F%E6%9C%BA%E8%8E%B7%E5%8F%96%E7%AC%91%E8%AF%9D%E6%AE%B5%E5%AD%90%E5%88%97%E8%A1%A8)
  - 八、生成二维码
    - [**生成单一二维码**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E7%94%9F%E6%88%90%E5%8D%95%E4%B8%80%E4%BA%8C%E7%BB%B4%E7%A0%81)
    - [**生成带logo二维码**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E7%94%9F%E6%88%90%E5%B8%A6logo%E4%BA%8C%E7%BB%B4%E7%A0%81)
  - 九、生成随机图片验证码
    - [**生成随机图片验证码**](https://juejin.im/post/5c062f0ce51d451d992a973a#%E7%94%9F%E6%88%90%E9%9A%8F%E6%9C%BA%E5%9B%BE%E7%89%87%E9%AA%8C%E8%AF%81%E7%A0%81)

------

## 通用

- **HOST地址：** **www.mxnzp.com/api**

- **说明：** 所有的接口都会返回如下格式的数据，具体数据包装在data中，需要根据状态来确定请求是否成功。

- **请求方法：** 所有的请求中都是大部分都是GET请求（如果有特殊情况，则会特殊标明）

- **数据返回格式：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": null
  }
  复制代码
  ```

- **数据返回格式说明（下面所有接口中的数据返回都是基于data的，不再介绍code和msg，请知悉）：**

  - **code：** 状态码 1 返回成功 0 返回失败 此时，请关注msg错误信息
  - **msg：** 提示信息，当code返回0的时候包含错误提示信息
  - **data：** 主要信息，不同接口返回的东西不一样

------

## 更新记录

**2019年01月09日20:16:38**

- 新增生成随机验证码的接口，可以生成任意长度的验证码图片，[查看说明](https://juejin.im/post/5c062f0ce51d451d992a973a#%E4%B9%9D%E7%94%9F%E6%88%90%E9%9A%8F%E6%9C%BA%E9%AA%8C%E8%AF%81%E7%A0%81)；段子接口新增随机段子列表，可以获取随机段子，[查看说明](https://juejin.im/post/5c062f0ce51d451d992a973a#%E9%9A%8F%E6%9C%BA%E8%8E%B7%E5%8F%96%E7%AC%91%E8%AF%9D%E6%AE%B5%E5%AD%90%E5%88%97%E8%A1%A8)。

**2018年12月14日15:02:00**

- 新增生成二维码的接口，可生成指定大小，指定内容的二位么，也可生成带logo的二维码。[查看说明](https://juejin.im/post/5c062f0ce51d451d992a973a#%E5%85%AB%E7%94%9F%E6%88%90%E4%BA%8C%E7%BB%B4%E7%A0%81)

**2018年12月10日22:54:46**

- 节假日及万年历接口添加新的接口，添加查询指定类型的节假日信息列表，比如节日，休息日，工作日 [查看说明](https://juejin.im/post/5c062f0ce51d451d992a973a#%E6%8C%87%E5%AE%9A%E6%9C%88%E4%BB%BD%E6%8C%87%E5%AE%9A%E7%B1%BB%E5%9E%8B%E7%9A%84%E6%89%80%E6%9C%89%E7%9A%84%E8%8A%82%E5%81%87%E6%97%A5%E5%8F%8A%E4%B8%87%E5%B9%B4%E5%8E%86%E4%BF%A1%E6%81%AF) 和 [查看说明](https://juejin.im/post/5c062f0ce51d451d992a973a#%E6%8C%87%E5%AE%9A%E5%B9%B4%E4%BB%BD%E6%8C%87%E5%AE%9A%E7%B1%BB%E5%9E%8B%E7%9A%84%E6%89%80%E6%9C%89%E7%9A%84%E8%8A%82%E5%81%87%E6%97%A5%E5%8F%8A%E4%B8%87%E5%B9%B4%E5%8E%86%E4%BF%A1%E6%81%AF)

**2018年12月07日09:20:07**

- 添加正式域名，可用正式域名访问 [查看说明](https://juejin.im/post/5c062f0ce51d451d992a973a#%E9%80%9A%E7%94%A8)

**2018年12月01日22:49:42**

- 新增笑话段子的api接口 [查看说明](https://juejin.im/post/5c062f0ce51d451d992a973a#%E4%B8%83%E7%AC%91%E8%AF%9D%E6%AE%B5%E5%AD%90)

**2018年11月27日23:14:49**

- 新增天气查询的api接口 [查看说明](https://juejin.im/post/5c062f0ce51d451d992a973a#%E5%85%AD%E5%A4%A9%E6%B0%94%E4%BF%A1%E6%81%AF)

------

## 接口列表

### 一、福彩-双色球接口

#### **指定期号中奖号码**

- **接口说明：** 获取指定期号的双色球获奖号码信息

- **接口地址：** [HOST]/lottery/ssq/aim_lottery?expect=2018135

- **参数说明：** expect：彩票期号（七位）必传

- **返回数据：**

  - **openCode：** 本期中奖号码
  - **code：** 彩票编号标识（双色球是ssq）
  - **expect：** 彩票期号
  - **name：** 彩票名称
  - **time：** 发布时间

- **数据样例：**

  ```
  {
      "openCode": "01,03,06,10,11,29+16",
      "code": "ssq",
      "expect": "2018135",
      "name": "双色球",
      "time": "2018-11-18 21:18:20"
  }
  复制代码
  ```

#### **最新中奖号码信息**

- **接口说明：** 获取最新双色球中奖号码信息

- **接口地址：** [HOST]/lottery/ssq/latest

- **参数说明：** 无

- **返回数据：**

  - **openCode：** 本期中奖号码
  - **code：** 彩票编号标识（双色球是ssq）
  - **expect：** 彩票期号
  - **name：** 彩票名称
  - **time：** 发布时间

- **数据样例：**

  ```
  {
      "openCode": "10,12,15,25,26,27+14",
      "code": "ssq",
      "expect": "2018136",
      "name": "双色球",
      "time": "2018-11-20 21:18:20"
  }
  复制代码
  ```

#### **获取双色球中奖信息列表**

- **接口说明：** 获取最新双色球中奖号码信息

- **接口地址：** [HOST]/lottery/ssq/lottery_list?page=1

- **参数说明：** page 页号

- **返回数据：**

  - **page：** 当前页数
  - **totalCount：** 总数量
  - **totalPage：** 总页数
  - **limit：** 每页数量
  - **list：** 每页具体数据
    - **openCode：** 本期中奖号码
    - **code：** 彩票编号标识（双色球是ssq）
    - **expect：** 彩票期号
    - **name：** 彩票名称
    - **time：** 发布时间

- **数据样例：**

  ```
  {
      "page": 1,
      "totalCount": 903,
      "totalPage": 91,
      "limit": 10,
      "list": [
          {
              "openCode": "10,12,15,25,26,27+14",
              "code": "ssq",
              "expect": "2018136",
              "name": "双色球",
              "time": "2018-11-20 21:18:20"
          },
          {
              "openCode": "01,03,06,10,11,29+16",
              "code": "ssq",
              "expect": "2018135",
              "name": "双色球",
              "time": "2018-11-18 21:18:20"
          }
      ]
  }
  复制代码
  ```

### 二、节假日及万年历

#### **指定日期的节假日及万年历信息**

------

**2018-11-26 18:07:28更新：** 节假日新增类型描述，比如【国庆,休息日,工作日】

------

- **接口说明：** 获取指定日期的节假日及万年历信息

- **接口地址：** [HOST]/holiday/single/{date} 【例如： [HOST]/holiday/single/20181121】

- **参数说明：** date 日期 格式 yyyyMMdd

- **返回数据：**

  - **date：** 当前日期
  - **weekDay：** 当前周第几天 1-周一 2-周二 ... 7-周日
  - **yearTips：** 天干地支纪年法描述 例如：戊戌
  - **type：** 类型 0 工作日 1 假日 2 节假日
  - **typeDes：** 类型描述 比如 国庆,休息日,工作日
  - **chineseZodiac：** 属相 例如：狗
  - **solarTerms：** 节气描述 例如：小雪
  - **lunarCalendar：** 农历日期
  - **suit：** 宜事项
  - **dayOfYear：** 这一年的第几天
  - **weekOfYear：** 这一年的第几周

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": {
          "date": "2018-11-25",
          "weekDay": 7,
          "yearTips": "戊戌",
          "type": 1,
          "chineseZodiac": "狗",
          "solarTerms": "小雪后",
          "typeDes" : "休息日",
          "avoid": "移徙.入宅.安门.作梁.安葬",
          "lunarCalendar": "10-18",
          "suit": "祭祀.祈福.求嗣.斋醮.沐浴.冠笄.出行.理发.拆卸.解除.起基.动土.定磉.安碓硙.开池.掘井.扫舍.除服.成服.移柩.启攒.立碑.谢土",
          "dayOfYear": 329,
          "weekOfYear": 47
      }
  }
  复制代码
  ```

#### **指定多个日期的节假日及万年历信息**

- **接口说明：** 获取指定多个日期的节假日及万年历信息

- **接口地址：** [HOST]/holiday/multi/{dates} 【例如： [HOST]/holiday/multi/20180101,20181010,20181011】

- **参数说明：** dates 日期组 格式 yyyyMMdd,yyyyMMdd,yyyyMMdd （中间用英文逗号隔开）

- **返回数据：**

  - **date：** 当前日期
  - **weekDay：** 当前周第几天 1-周一 2-周二 ... 7-周日
  - **yearTips：** 天干地支纪年法描述 例如：戊戌
  - **type：** 类型 0 工作日 1 假日 2 节假日
  - **typeDes：** 类型描述 比如 国庆,休息日,工作日
  - **chineseZodiac：** 属相 例如：狗
  - **solarTerms：** 节气描述 例如：小雪
  - **lunarCalendar：** 农历日期
  - **suit：** 宜事项
  - **dayOfYear：** 这一年的第几天
  - **weekOfYear：** 这一年的第几周

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": [
          {
              "date": "2018-01-01",
              "weekDay": 1,
              "yearTips": "丁酉",
              "type": 2,
              "chineseZodiac": "鸡",
              "solarTerms": "冬至后",
              "avoid": "出行.安葬.修坟.开市",
              "lunarCalendar": "11-15",
              "typeDes" : "元旦",
              "suit": "祭祀.塑绘.开光.裁衣.冠笄.嫁娶.纳采.拆卸.修造.动土.竖柱.上梁.安床.移徙.入宅.安香.结网.捕捉.畋猎.伐木.进人口.放水",
              "dayOfYear": 1,
              "weekOfYear": 1
          },
          {
              "date": "2018-10-10",
              "weekDay": 3,
              "yearTips": "戊戌",
              "type": 0,
              "chineseZodiac": "狗",
              "solarTerms": "寒露后",
              "typeDes" : "工作日",
              "avoid": "造庙.嫁娶.掘井.栽种.造桥.作灶.动土",
              "lunarCalendar": "9-2",
              "suit": "祭祀.开光.出行.解除.伐木.作梁.出火.拆卸.入宅.移徙.安床.修造.造畜椆栖.扫舍",
              "dayOfYear": 283,
              "weekOfYear": 41
          },
          {
              "date": "2018-10-11",
              "weekDay": 4,
              "yearTips": "戊戌",
              "type": 0,
              "typeDes" : "工作日",
              "chineseZodiac": "狗",
              "solarTerms": "寒露后",
              "avoid": "入宅.上梁.斋醮.出火.谢土",
              "lunarCalendar": "9-3",
              "suit": "纳采.订盟.开市.交易.立券.会亲友.纳畜.牧养.问名.移徙.解除.作厕.入学.起基.安床.开仓.出货财.安葬.启攒.入殓.除服.成服",
              "dayOfYear": 284,
              "weekOfYear": 41
          }
      ]
  }
  复制代码
  ```

#### **指定月份所有的节假日及万年历信息**

- **接口说明：** 获取指定月份的节假日及万年历信息

- **接口地址：** [HOST]/holiday/list/month/{date} 【例如： [HOST]/holiday/list/month/201802】

- **参数说明：** date 查询的月份 格式 yyyyMM （只有年月）

- **返回数据：**

  - **date：** 当前日期
  - **weekDay：** 当前周第几天 1-周一 2-周二 ... 7-周日
  - **yearTips：** 天干地支纪年法描述 例如：戊戌
  - **type：** 类型 0 工作日 1 假日 2 节假日
  - **typeDes：** 类型描述 比如 国庆,休息日,工作日
  - **chineseZodiac：** 属相 例如：狗
  - **solarTerms：** 节气描述 例如：小雪
  - **lunarCalendar：** 农历日期
  - **suit：** 宜事项
  - **dayOfYear：** 这一年的第几天
  - **weekOfYear：** 这一年的第几周

- **数据样例：**

  ```
  {

      "code": 1,
      "msg": "数据返回成功",
      "data": [
          {
              "date": "2018-02-01",
              "weekDay": 4,
              "yearTips": "丁酉",
              "type": 0,
              "chineseZodiac": "鸡",
              "typeDes" : "工作日",
              "solarTerms": "大寒后",
              "avoid": "开仓.嫁娶.移徙.入宅",
              "lunarCalendar": "12-16",
              "suit": "祭祀.沐浴.祈福.斋醮.订盟.纳采.裁衣.拆卸.起基.竖柱.上梁.安床.入殓.除服.成服.移柩.启攒.挂匾.求嗣.出行.合帐.造畜椆栖",
              "dayOfYear": 32,
              "weekOfYear": 5
          },
          ...中间隐藏了"2018-02-02"~"2018-02-27"的数据
          {
              "date": "2018-02-28",
              "weekDay": 3,
              "yearTips": "戊戌",
              "type": 0,
              "chineseZodiac": "狗",
              "typeDes" : "工作日",
              "solarTerms": "雨水后",
              "avoid": "掘井",
              "lunarCalendar": "1-13",
              "suit": "祭祀.斋醮.裁衣.合帐.冠笄.订盟.纳采.嫁娶.入宅.安香.谢土.入殓.移柩.破土.立碑.安香.会亲友.出行.祈福.求嗣.立碑.上梁.放水",
              "dayOfYear": 59,
              "weekOfYear": 9
          }
      ]

  }
  复制代码
  ```

#### **指定月份指定类型的所有的节假日及万年历信息**

- **接口说明：** 获取指定月份的节假日及万年历信息

- **接口地址：** [HOST]/holiday/list/month/{date}/{type} 【例如： [HOST]/holiday/list/month/201810/rest】

- **参数说明：** date 查询的月份 格式 yyyyMM （只有年月），type 需要查询的类型{可选值：类型 workday 工作日 holiday 节假日 rest 休息日 festival 节日}

- **返回数据：**

  - **month：** 当前月份

  - year：

     

    当前年份

    - **date：** 当前日期
    - **weekDay：** 当前周第几天 1-周一 2-周二 ... 7-周日
    - **yearTips：** 天干地支纪年法描述 例如：戊戌
    - **type：** 类型 0 工作日 1 假日 2 节假日
    - **typeDes：** 类型描述 比如 国庆,休息日,工作日
    - **chineseZodiac：** 属相 例如：狗
    - **solarTerms：** 节气描述 例如：小雪
    - **lunarCalendar：** 农历日期
    - **suit：** 宜事项
    - **dayOfYear：** 这一年的第几天
    - **weekOfYear：** 这一年的第几周

- 数据样例

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": [
          {
              "month": 10,
              "year": 2018,
              "days": [
                  {
                      "date": "2018-10-13",
                      "weekDay": 6,
                      "yearTips": "戊戌",
                      "type": 1,
                      "typeDes": "休息日",
                      "chineseZodiac": "狗",
                      "solarTerms": "寒露后",
                      "avoid": "开市.交易.祭祀.入宅.安葬",
                      "lunarCalendar": "九月初五",
                      "suit": "捕捉.畋猎.余事勿取",
                      "dayOfYear": 286,
                      "weekOfYear": 41
                  },
                  ...中间隐藏了一部分的数据...
                  {
                      "date": "2018-10-28",
                      "weekDay": 7,
                      "yearTips": "戊戌",
                      "type": 1,
                      "typeDes": "休息日",
                      "chineseZodiac": "狗",
                      "solarTerms": "霜降后",
                      "avoid": "出行.祈福.安葬.作灶",
                      "lunarCalendar": "九月廿",
                      "suit": "会亲友.嫁娶.订盟.纳采.纳婿.拆卸.修造.动土.起基.竖柱.上梁.安床.会亲友.纳财",
                      "dayOfYear": 301,
                      "weekOfYear": 43
                  }
              ]
          }
      ]
  }
  复制代码
  ```

#### **指定年份所有的节假日及万年历信息**

- **接口说明：** 获取指定年份的节假日及万年历信息

- **接口地址：** [HOST]/holiday//list/year/{date} 【例如： [HOST]/holiday/list/year/2018】

- **参数说明：** date 查询的年份 格式 yyyy （只有年份）

- **返回数据：**

  - **month：** 当前月份

  - year：

     

    当前年份

    - **date：** 当前日期
    - **weekDay：** 当前周第几天 1-周一 2-周二 ... 7-周日
    - **yearTips：** 天干地支纪年法描述 例如：戊戌
    - **type：** 类型 0 工作日 1 假日 2 节假日
    - **typeDes：** 类型描述 比如 国庆,休息日,工作日
    - **chineseZodiac：** 属相 例如：狗
    - **solarTerms：** 节气描述 例如：小雪
    - **lunarCalendar：** 农历日期
    - **suit：** 宜事项
    - **dayOfYear：** 这一年的第几天
    - **weekOfYear：** 这一年的第几周

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": [
          {
              "month": 1,
              "year": 2018,
              "days": [
                  {
                      "date": "2018-01-01",
                      "weekDay": 1,
                      "yearTips": "丁酉",
                      "type": 2,
                      "chineseZodiac": "鸡",
                      "solarTerms": "冬至后",
                      "typeDes" : "元旦",
                      "avoid": "出行.安葬.修坟.开市",
                      "lunarCalendar": "11-15",
                      "suit": "祭祀.塑绘.开光.裁衣.冠笄.嫁娶.纳采.拆卸.修造.动土.竖柱.上梁.安床.移徙.入宅.安香.结网.捕捉.畋猎.伐木.进人口.放水",
                      "dayOfYear": 1,
                      "weekOfYear": 1
                  },
                  ...中间隐藏了"2018-01-02"~"2018-01-30"的数据
                  {
                      "date": "2018-01-31",
                      "weekDay": 3,
                      "yearTips": "丁酉",
                      "type": 0,
                      "chineseZodiac": "鸡",
                      "typeDes" : "工作日",
                      "solarTerms": "大寒后",
                      "avoid": "嫁娶.入殓.安葬.出行",
                      "lunarCalendar": "12-15",
                      "suit": "塑绘.开光.沐浴.冠笄.会亲友.作灶.放水.造畜椆栖",
                      "dayOfYear": 31,
                      "weekOfYear": 5
                  }
              ]
          },
          ...中间隐藏了02月到11月的数据
          {
              "month": 12,
              "days": [
                  {
                      "date": "2018-12-01",
                      "weekDay": 6,
                      "yearTips": "戊戌",
                      "type": 1,
                      "chineseZodiac": "狗",
                      "typeDes" : "休息日",
                      "solarTerms": "小雪后",
                      "avoid": "作灶.治病",
                      "lunarCalendar": "10-24",
                      "suit": "祭祀.祈福.订盟.纳采.裁衣.拆卸.修造.动土.起基.安床.移徙.入宅.安香.入殓.移柩.安葬.谢土.赴任.进人口.会亲友",
                      "dayOfYear": 335,
                      "weekOfYear": 48
                  },
                  ...中间隐藏了"2018-12-02"~"2018-12-30"的数据
                  {
                      "date": "2018-12-31",
                      "weekDay": 1,
                      "yearTips": "戊戌",
                      "type": 0,
                      "chineseZodiac": "狗",
                      "solarTerms": "冬至后",
                      "avoid": "开市.破土",
                      "lunarCalendar": "10-25",
                      "suit": "祭祀.沐浴.安床.纳财.畋猎.捕捉",
                      "dayOfYear": 365,
                      "weekOfYear": 1
                  }
              ]
          }
      ]
  }
  复制代码
  ```

#### **指定年份指定类型的所有的节假日及万年历信息**

- **接口说明：** 获取指定月份的节假日及万年历信息

- **接口地址：** [HOST]/holiday/list/year/{date}/{type} 【例如： [HOST]/holiday/list/year/2018/rest】

- **参数说明：** date 查询的月份 格式 yyyy （只有年份），type 需要查询的类型{可选值：类型 workday 工作日 holiday 节假日 rest 休息日 festival 节日}

- **返回数据：**

  - **month：** 当前月份

  - year：

     

    当前年份

    - **date：** 当前日期
    - **weekDay：** 当前周第几天 1-周一 2-周二 ... 7-周日
    - **yearTips：** 天干地支纪年法描述 例如：戊戌
    - **type：** 类型 0 工作日 1 假日 2 节假日
    - **typeDes：** 类型描述 比如 国庆,休息日,工作日
    - **chineseZodiac：** 属相 例如：狗
    - **solarTerms：** 节气描述 例如：小雪
    - **lunarCalendar：** 农历日期
    - **suit：** 宜事项
    - **dayOfYear：** 这一年的第几天
    - **weekOfYear：** 这一年的第几周

- 数据样例

  ```
  {
      "code": 1,
      "msg": "数据返回成功，域名已经成功备案，为了更优雅的调用，不久后将废弃8091端口，请尽快使用新域名直接调用，多有不便敬请谅解",
      "data": [
          {
              "month": 1,
              "year": 2018,
              "days": [
                  {
                      "date": "2018-01-06",
                      "weekDay": 6,
                      "yearTips": "丁酉",
                      "type": 1,
                      "typeDes": "休息日",
                      "chineseZodiac": "鸡",
                      "solarTerms": "小寒后",
                      "avoid": "嫁娶.开市.入宅.安床.破土.安葬",
                      "lunarCalendar": "冬月廿",
                      "suit": "祭祀.斋醮.纳财.捕捉.畋猎",
                      "dayOfYear": 6,
                      "weekOfYear": 1
                  },
                  ...中间还有一些数据没有显示...
                  {
                      "date": "2018-01-28",
                      "weekDay": 7,
                      "yearTips": "丁酉",
                      "type": 1,
                      "typeDes": "休息日",
                      "chineseZodiac": "鸡",
                      "solarTerms": "大寒后",
                      "avoid": "祈福.嫁娶.造庙.安床.谢土",
                      "lunarCalendar": "腊月十二",
                      "suit": "纳采.订盟.祭祀.求嗣.出火.塑绘.裁衣.会亲友.入学.拆卸.扫舍.造仓.挂匾.掘井.开池.结网.栽种.纳畜.破土.修坟.立碑.安葬.入殓",
                      "dayOfYear": 28,
                      "weekOfYear": 4
                  }
              ]
          },
          ...中间有2月到11月的数据没有展示...
          {
              "month": 12,
              "year": 2018,
              "days": [
                  {
                      "date": "2018-12-01",
                      "weekDay": 6,
                      "yearTips": "戊戌",
                      "type": 1,
                      "typeDes": "休息日",
                      "chineseZodiac": "狗",
                      "solarTerms": "小雪后",
                      "avoid": "作灶.治病",
                      "lunarCalendar": "十月廿四",
                      "suit": "祭祀.祈福.订盟.纳采.裁衣.拆卸.修造.动土.起基.安床.移徙.入宅.安香.入殓.移柩.安葬.谢土.赴任.进人口.会亲友",
                      "dayOfYear": 335,
                      "weekOfYear": 48
                  },
                  ...中间还有一些数据没有显示...
                  {
                      "date": "2018-12-30",
                      "weekDay": 7,
                      "yearTips": "戊戌",
                      "type": 1,
                      "typeDes": "元旦",
                      "chineseZodiac": "狗",
                      "solarTerms": "冬至后",
                      "avoid": null,
                      "lunarCalendar": "冬月廿四",
                      "suit": "塑绘.斋醮.出行.拆卸.解除.修造.移徙.造船.入殓.除服.成服.移柩.启攒.修坟.立碑.谢土",
                      "dayOfYear": 364,
                      "weekOfYear": 52
                  }
              ]
          }
      ]
  }
  复制代码
  ```

### 三、全国城市列表（全国地级市API，数据来源国家统计局）

#### **全国城市列表**

- **接口说明：** 获取全国城市列表信息

- **接口地址：** [HOST]/address/list

- **参数说明：** 无参

- **返回数据：**

  - **code：** 省/市/区编号
  - **name：** 省/市/区名称
  - **pchilds：** 市列表
  - **cchilds：** 区列表

- **数据样例：**

  ```
  {
      "code":1,
      "msg":"数据返回成功",
      "data":[
          {
              "code":"130000",
              "name":"河北省",
              "pchilds":[
                  {
                      "code":"130100",
                      "name":"石家庄市",
                      "cchilds":[
                          {
                              "code":"130101",
                              "name":"市辖区"
                          },
                          {
                              "code":"130102",
                              "name":"长安区"
                          },
                          ...这里只显示了两个区...
                      ]
                  },
                  {
                      "code":"130200",
                      "name":"唐山市",
                      "cchilds":[
                          {
                              "code":"130201",
                              "name":"市辖区"
                          },
                          {
                              "code":"130202",
                              "name":"路南区"
                          },
                          ...这里只显示了两个区...
                      ]
                  },
                  ...这里只显示了两个市...
              ]
          }
          ...这里只显示了一个省...
      ]
  }
  复制代码
  ```

#### **搜索全国城市列表**

- **接口说明：** 搜索全国城市列表信息

- **接口地址：** [HOST]/address/search 【例如： [HOST]/address/search?type=1&value=深圳】

- **参数说明：**

  - **type：** 类型 0-查询省份 1-查询城市
  - **value：** 被查询的省份或者城市名称

- **返回数据：**

  - **code：** 省/市/区编号
  - **name：** 省/市/区名称
  - **pchilds：** 市列表
  - **cchilds：** 区列表

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": [
          {
              "code": "440000",
              "name": "广东省",
              "pchilds": [
                  {
                      "code": "440300",
                      "name": "深圳市",
                      "cchilds": [
                          {
                              "code": "440301",
                              "name": "市辖区"
                          },
                          {
                              "code": "440303",
                              "name": "罗湖区"
                          },
                          ...这里只显示了两个区...
                      ]
                  }
              ]
          }
      ]
  }
  复制代码
  ```

### 四、IP地址信息

#### **获取访问者的ip地址信息**

- **接口说明：** 获取访问者的ip地址信息，先获取您的ip地址，再进行解析

- **接口地址：** [HOST]/ip/self

- **参数说明：** 无参

- **返回数据：**

  - **ip：** 访问者的ip地址
  - **province：** 省份
  - **provinceId：** 省份id
  - **city：** 城市
  - **cityId：** 城市id
  - **isp：** 网络服务商名称 例如 电信
  - **desc：** 拼接好的描述信息

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": {
          "ip": "119.123.72.166",
          "province": "广东省",
          "provinceId": 440000,
          "city": "深圳市",
          "cityId": 440300,
          "isp": "电信",
          "desc": "广东省深圳市 电信"
      }
  }
  复制代码
  ```

#### **获取指定ip的ip地址信息**

- **接口说明：** 获取指定ip的ip地址信息

- **接口地址：** [HOST]/ip/aim_ip?ip=? 【例如： [HOST]/ip/aim_ip?ip=119.123.72.166】

- **参数说明：** ip 被查询的ip地址 需保证是正确的ip地址格式

- **返回数据：**

  - **ip：** 访问者的ip地址
  - **province：** 省份
  - **provinceId：** 省份id
  - **city：** 城市
  - **cityId：** 城市id
  - **isp：** 网络服务商名称 例如 电信
  - **desc：** 拼接好的描述信息

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": {
          "ip": "119.123.72.166",
          "province": "广东省",
          "provinceId": 440000,
          "city": "深圳市",
          "cityId": 440300,
          "isp": "电信",
          "desc": "广东省深圳市 电信"
      }
  }
  复制代码
  ```

### 五、小工具

#### **获取不重复长ID**

- **接口说明：** 获取不重复长ID信息

- **接口地址：** [HOST]/tools/no_repeat_id/long

- **参数说明：** 无参

- **返回数据：**

  - **id：** 不重复16位字符id

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": {
          "id": "8a2a789976e64a1c9455ebd90853d4c6"
      }
  }
  复制代码
  ```

#### **获取不重复短ID**

- **接口说明：** 获取不重复短ID信息

- **接口地址：** [HOST]/tools/no_repeat_id/short

- **参数说明：** 无参

- **返回数据：**

  - **id：** 不重复8位字符id

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": {
          "id": "jlazntmtjrvcrpnb"
      }
  }
  复制代码
  ```

### 六、天气信息

#### **获取特定城市今日天气**

- **接口说明：** 获取特定城市今日天气信息

- **接口地址：** [HOST]/weather/current/{城市名} 【例如： [HOST]/weather/current/深圳市】

- **参数说明：** {城市名} 传入你需要查询的城市，请尽量传入完整值，否则系统会自行匹配，可能会有误差

- **返回数据：**

  - **address：** 城市具体信息，比如 “广东省 深圳市”
  - **cityCode：** 城市code
  - **temp：** 温度值
  - **weather：** 天气描述
  - **windDirection：** 风向描述
  - **windPower：** 风力描述
  - **humidity：** 湿度值
  - **reportTime：** 此次天气发布时间

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": {
          "address": "广东省 深圳市",
          "cityCode": "440300",
          "temp": "18℃",
          "weather": "小雨",
          "windDirection": "东北",
          "windPower": "≤3级",
          "humidity": "92%",
          "reportTime": "2018-11-27 22:40:53"
      }
  }
  复制代码
  ```

#### **获取特定城市今天及未来天气**

- **接口说明：** 获取特定城市今天及未来天气信息

- **接口地址：** [HOST]/weather/forecast/{城市名} 【例如： [HOST]/weather/forecast/深圳市】

- **参数说明：** {城市名} 传入你需要查询的城市，请尽量传入完整值，否则系统会自行匹配，可能会有误差

- **返回数据：**

  - **address：** 城市具体信息，比如 “广东省 深圳市”

  - **cityCode：** 城市code

  - **reportTime：** 此次天气发布时间

  - forecasts：

     

    今天及未来天气列表

    - **date：** 日期
    - **dayOfWeek：** 星期
    - **dayWeather：** 白天天气描述
    - **nightWeather：** 晚上天气描述
    - **dayTemp：** 白天温度
    - **nightTemp：** 晚上温度
    - **dayWindDirection：** 白天风向
    - **nightWindDirection：** 晚上风向
    - **dayWindPower：** 白天风力
    - **nightWindPower：** 晚上风力

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": {
          "address": "广东省 深圳市",
          "cityCode": "440300",
          "reportTime": "2018-11-27 22:40:53",
          "forecasts": [
              {
                  "date": "2018-11-27",
                  "dayOfWeek": "2",
                  "dayWeather": "阵雨",
                  "nightWeather": "小雨",
                  "dayTemp": "22℃",
                  "nightTemp": "17℃",
                  "dayWindDirection": "无风向",
                  "nightWindDirection": "无风向",
                  "dayWindPower": "≤3级",
                  "nightWindPower": "≤3级"
              },
              ...这里只显示了一条数据...
          ]
      }
  }
  复制代码
  ```

### 七、笑话段子

#### **分页获取笑话段子列表**

- **特别说明：** 此接口的数据来源是我的另外一个产品【段子乐】，目前Android客户端已经在各大应用市场上架，定期更新数据到此服务。本服务目前只开放纯文本段子，后期看情况开放搞笑短视频和搞笑图片的接口。

- **接口说明：** 分页获取笑话段子列表

- **接口地址：** [HOST/jokes/list 【例如： [HOST]/jokes/list?page=1】

- **参数说明：** page 分页

- **返回数据：**

  - **page：** 当前页数

  - **totalCount：** 总数量

  - **totalPage：** 总页数

  - **limit：** 每页数量

  - list：

     

    每页具体数据

    - **content：** 段子内容
    - **updateTime：** 更新时间

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": {
          "page": 2,
          "totalCount": 9590,
          "totalPage": 959,
          "limit": 10,
          "list": [
              {
                  "content": "儿子:“爸爸，为什么王叔叔那么喜欢吃辣”爸爸:“你怎么知道王叔叔喜欢吃辣？”儿子:“别人都叫我妈妈为辣妈，我经常看到王叔叔抱着我妈妈又亲又啃”爸爸:“尼玛”",
                  "updateTime": "2018-11-03 09:45:28"
              },
              ...这里只显示了一条数据...
          ]
      }
  }
  复制代码
  ```

#### **随机获取笑话段子列表**

- **特别说明：** 此接口的数据来源是我的另外一个产品【段子乐】，目前Android客户端已经在各大应用市场上架，定期更新数据到此服务。本服务目前只开放纯文本段子，后期看情况开放搞笑短视频和搞笑图片的接口。

- **接口说明：** 随机获取笑话段子列表

- **接口地址：** [HOST/jokes/list/random 【例如： [HOST]/jokes/list/random】

- **参数说明：** 无参

- **返回数据：**

  - **content：** 段子内容
  - **updateTime：** 更新时间

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": [
          {
              "content": "朋友问我，如果在这个时代做个普通人，你最想做什么样的。我说，我想做个皇城根底下的社会闲散人员，好吃懒做，游手好闲，靠着祖上的余荫收点租子过日子。",
              "updateTime": "2018-04-30 13:45:44"
          },
          ...这里只显示了一条数据...
      ]
  }
  复制代码
  ```

### 八、生成二维码

#### **生成单一二维码**

- **接口说明：** 根据传入的内容生成二维码，可以选择获取二维码下载链接，也可以直接获取图片的Base64字符串自己解析（注：Base64字符串前面默认添加了“data:image/jpg;base64,”，取值的时候请根据需要对这个内容进行处理）。

- **接口地址：** [HOST]/qrcode/create/single 【例如： [HOST]/qrcode/create/single?content=你好&size=500&type=0】

- **参数说明：** content：生成二维码的内容 size：生成二维码的大小（不传默认为500）type：你希望返回二维码的类型：（0=下载链接 1=base64字符串）

- **返回数据：**

  - **qrCodeUrl：** 如果type=0 则此参数会有值，且此值会返回二维码的下载链接
  - **content：** 此二维码所代表的内容
  - **type：** 生成的二维码的输出方式 （0=下载链接 1=base64字符串）
  - **qrCodeBase64：** 如果type=1 则此参数会有值，且此值会返回二维码的base64字符串（注：Base64字符串前面默认添加了“data:image/jpg;base64,”，取值的时候请根据需要对这个内容进行处理）

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": {
          "qrCodeUrl": "http://www.mxnzp.com/api_file/qrcode/7/2/d/d/0/9/a/e/327588b1ddb44cf7a95e43d7ad2f5b90.png",
          "content": "你好",
          "type": 0,
          "qrCodeBase64": null
      }
  }
  复制代码
  ```

#### **生成带logo二维码**

- **接口说明：** 根据传入的内容生成带logo的二维码，可以选择获取二维码下载链接，也可以直接获取图片的Base64字符串自己解析（注：Base64字符串前面默认添加了“data:image/jpg;base64,”，取值的时候请根据需要对这个内容进行处理）。

- **请求方法：** POST

- **接口地址：** [HOST]/qrcode/create/logo 【例如： [HOST]/qrcode/create/logo?content=你好&size=600&logo_size=500&type=0&logo_img=logo图片】

- **参数说明：** content：生成二维码的内容 size：生成二维码的大小（不传默认为500）type：你希望返回二维码的类型：（0=下载链接 1=base64字符串） logo_size：logo的大小（不传默认为而二维码大小的1/5）logo_img：嵌入在二维码中的logo图片文件，使用post请求上传至服务器

- **返回数据：**

  - **qrCodeUrl：** 如果type=0 则此参数会有值，且此值会返回二维码的下载链接
  - **content：** 此二维码所代表的内容
  - **type：** 生成的二维码的输出方式 （0=下载链接 1=base64字符串）
  - **qrCodeBase64：** 如果type=1 则此参数会有值，且此值会返回二维码的base64字符串（注：Base64字符串前面默认添加了“data:image/jpg;base64,”，取值的时候请根据需要对这个内容进行处理）

- **数据样例：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": {
          "qrCodeUrl": "http://127.0.0.1:8080/api_file/qrcode/d/8/f/7/3/a/1/8/ff1ea758421647ca9f96136f6298aac8.png",
          "content": "你好",
          "type": 0,
          "qrCodeBase64": null
      }
  }
  复制代码
  ```

### 九、生成随机图片验证码

#### **生成随机图片验证码**

- **接口说明：** 生成随机长度的图片验证码。

- **接口地址：** [HOST]/verifycode/code 【例如： [HOST]/verifycode/code?len=5&type=0】

- **参数说明：** len：生成验证码的长度，不传默认5位，type=0：返回类型，0-生成图片的地址链接 1-生成验证码图片的base64字符串（注：Base64字符串前面默认添加了“data:image/jpg;base64,”，取值的时候请根据需要对这个内容进行处理）

- **返回数据：**

  - **verifyCode：** 图片对应的验证码的值
  - **verifyCodeImgUrl：** 如果type=0 则此参数会有值，且此值会返回验证码的下载链接
  - **verifyCodeBase64：** 如果type=1 则此参数会有值，且此值会返回验证码的base64字符串（注：Base64字符串前面默认添加了“data:image/jpg;base64,”，取值的时候请根据需要对这个内容进行处理）
  - **whRatio：** 验证码图片大小的宽高比

- **返回数据：**

  ```
  {
      "code": 1,
      "msg": "数据返回成功",
      "data": {
          "verifyCode": "jcyJG",
          "verifyCodeImgUrl": "http://127.0.0.1:8080/api_file/varitycode/a/6/d/a/9/8/c/2/592da008c864486396b9a2a68110d05e.jpg",
          "verifyCodeBase64": null,
          "whRatio": "225,80"
      }
  }
  复制代码
  ```

# **关于我的**

我就是比较喜欢用代码解决生活中的问题，感觉很开心，哈哈哈。也希望大家关注我的简书，掘金，Github和CSDN。

**简书首页**，链接是 [www.jianshu.com/u/123f97613…](https://www.jianshu.com/u/123f97613b86)

**掘金首页**，链接是 [juejin.im/user/5838d5…](https://juejin.im/user/5838d57fac502e006c1708bc)

**Github首页**，链接是 [github.com/MZCretin](https://github.com/MZCretin)

**CSDN首页**，链接是 [blog.csdn.net/u010998327](http://blog.csdn.net/u010998327)

我是Cretin，一个可爱的小男孩。

关注下面的标签，发现更多相似文章

[.NET](https://juejin.im/tag/.NET)