# Meumy直播弹幕数据展示页

本项目分为静态网页部分和数据处理服务器两部分，纯业余。

## 静态网页
使用了bootstrap5框架、`vue.js`和`echart.js`。

## 数据处理服务器
### live.py
使用了`bilibili-api`这个包把直播弹幕数据记录到sqlite数据库中，可以同时监听多个直播间的数据。
### Msg_db.py
一个记录弹幕、礼物、直播时间信息的sqlite数据库的简易类。会把每天的数据分别存到不同的文件中。
### danmu_analyse.py
每十分钟扫描今日的弹幕数据库，如果发现已开播就对弹幕进行分析，生成用于网页显示的json数据文件。

* 使用`scipy`包对弹幕量进行平滑以及峰值识别
* 使用`jieba`包进行中文分词和关键词提取
    * 用正则表达式过滤掉一些垃圾弹幕，比如ohhhhhhhh
    * 其中使用自定义词典`user_dict.txt`对分词进行了一点点优化
    * 分词部分还需要调整，有些词无法识别，需要经常维护自定义词典
    * 关键词识别不太适用于网络用语较多的环境，关键词全是“哈哈哈”，我也没啥办法
* 用正则表达式识别打call弹幕，从而识别出up唱歌的时段