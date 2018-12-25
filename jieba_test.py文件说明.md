# `jieba_test.py`文件说明

- package安装

  ```python
  import jieba.analyse
  import csv
  from wordcloud import WordCloud
  ```

- 读取数据

  ```python
  file = open("text.csv","r",encoding="utf-8")
  # 打开你的文件，csv格式
  count = 0
  # 用来去掉第一行表头
  textlist = []
  text = ""
  content = csv.reader(file)
  for row in content:
      if count != 0:
          textlist.append(row[2])
          text += row[2]
      count += 1
  # 上面的2表示评论是在第三列，自行修改
  file.close()
  f = open("stopwords.txt", "r")
  # 这个是标点符号的文件
  stopwords={}.fromkeys(f.read().split("\n"))
  f.close()
  ```

  - 最终目的是把你要分析的文字存到`text`和`testlist`里面，其实只要`test`就够了，`text`是把所有评价直接连在一起的字符串，`textlist`是储存每一个评论的list
  - 经过上面的过程，你会得到一个`text`和`stopwords`

- 分词

  ```python
  # method1
  tags = jieba.analyse.extract_tags(text, topK = 20,withWeight = True, allowPOS = ())
  for tag in tags:
      print("tag:%s\t\t weight:%f" % (tag[0], tag[1]))
  
  # 上面的代码和下面的区别在于是不是显示权重
  
  tags = jieba.analyse.extract_tags(text, topK = 20,withWeight = False, allowPOS = ())
  print("/".join(tags))
  
  # method2
  tags = jieba.analyse.textrank(text)
  print("/".join(tags))
  ```

  - 方法1和方法2使用的是不同的排序方法，详情可以查询`jieba`的算法说明，查`jieba`函数名字应该会有算法的说明
  - 这些代码之后就可以得到排序

- 词云

  ```python
  segs = jieba.cut(text)
  # 分词
  temp = []
  for seg in segs:
      if seg not in stopwords:
          temp.append(seg)
  cloud_text = ",".join(temp)
  # print(cloud_text)
  wc = WordCloud(
      background_color="white", #背景颜色
      max_words=200, #显示最大词数
      font_path="C:/Windows/Fonts/msyh.ttf",  #使用字体
      min_font_size=15,
      max_font_size=50,
      width=400  #图幅宽度
      )
  # 下面是保存图片
  wc.generate(cloud_text)
  wc.to_file("pic.png")
  ```

  - 这部分相当于是可视化
  - 算法可查wordcloud相关资料