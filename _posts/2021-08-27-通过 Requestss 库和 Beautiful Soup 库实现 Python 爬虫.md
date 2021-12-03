---
title: 通过 Requestss 库和 BeautifulSoup 库实现 Python 爬虫
tags: Python Requestss BeautifulSoup JSON YAML
---


参考了 MOOC 的 Python 爬虫课程，详细介绍了定向网络数据爬取技术和网络解析技术，开发了中国大学排名爬虫、淘宝商品搜索爬虫和股票数据爬虫三个定向爬虫。

<!--more-->  

## 一、Requests 库

第三方库，自动爬取HTML页面，自动网络请求提交  

有7个主要方法：  

![](/assets/image_Spider/requests库的7个主要方法.png)

### 1. **```requests.get(url)```**  
   
   返回 Response 对象，Response 对象属性：  

   ![](/assets/image_Spider/Response对象属性.png)  

   ```r.encoding```:如果header 中不存在charset,则认为编码为ISO-8859-1
### 2. Requests 库的异常  
   
   ![](/assets/image_Spider/requests库的异常.png)  

### 3. 爬取网页的通用代码框架  
   
   ```python
   import requests

   def getHTMLText(url):
      try:
         r = requests.get(url, timeout=30)
         r.raise_for_status() # 如果状态不是200.引发HTTPError异常
         r.encoding = r.apparent_encoding
         return r.text
      except:
         return "产生异常"

   if __name__ == "__main__":
      url = "http://www.baidu.com"
      print(getHTMLText(url))
   ```
### 4. Http协议与Requests 库
   
   ![](/assets/image_Spider/http%20协议与requests库.png)

### 5. ```requests.requests(method, url, **kwargs)```  
   
   **kwargs： 访问控制参数，均为可选项:  

   |         |         |         |      |
   |  ----   | ----    | ----    | ---- |
   | params  | cookies |proxies  |cert  |
   | data    | auth    |allow_redirects|
   |json     |files    |stream |
   |headers  |timeout  |verify  |

### 6. 网络爬虫尺寸  
   
   ![](/assets/image_Spider/网络爬虫尺寸.png)

## 二、robots.txt

网络爬虫爬出标准

## 实例

### 1. 爬取京东商品页面

   ```python
   import requests

   try:
      url = 'https://item.jd.com/100010058141.html'
      kv = {'user-agent': 'Mozilla/5.0'}
      r = requests.get(url, headers=kv)  # 模拟浏览器对服务器提出 http 请求
      r.raise_for_status()
      r.encoding = r.apparent_encoding
      print(type(r), r.status_code)
      print(r.text[:1000])
   except:
      print('爬取失败')
   ```

### 2. 360搜索
   
   ```python
   import requests

   try:
      keyword = 'python'
      url = 'http://www.so.com/s'
      kv_item = {'q': keyword}
      kv_header = {'user-agent': 'Mozilla/5.0'}
      r = requests.get(url, headers=kv_header, params=kv_item)
      print(r.request.url)
      r.raise_for_status()
      print(type(r), r.status_code)
      print(len(r.text))
   except:
      print('爬取失败')
   ```

### 3. 图片爬取
   
   ```python
   import requests
      import os

      url = 'https://img.natgeomedia.com/userfiles/sm/sm1920_/assets/image_Spiders_A1/14068/2021081937117737.jpg'
      root = 'E://pics//'
      path = root + url.split('/')[-1]
      try:
         if not os.path.exists(root):    # 文件夹如果不存在就新建一个
            os.mkdir(root)
         if not os.path.exists(path):    # 如果文件不存在才执行
            r = requests.get(url)
            print(r.status_code)
            r.raise_for_status()
            with open(path, 'wb') as f:
                  f.write(r.content)
                  f.close()
                  print('文件保存成功')
         else:
            print('文件已存在')
      except:
         print('爬取失败')
   ```

### 4. 在 ip138上查询ip 地址
   
   ```python
   import requests

      url = 'https://www.ip138.com/iplookup.asp'
      ip= '202.204.80.112'
      kv_ip = {'ip': ip, 'action': 2}
      kv_header = {'user-agent': 'Mozilla/5.0'}
      try:
         r = requests.get(url, headers=kv_header, params=kv_ip)
         print(r.request.url)
         r.raise_for_status()
         r.encoding = r.apparent_encoding
         print(r.text[1000:2000])
      except:
         print('爬取失败')
   ```

## 三、Beautiful Soup

解析HTML 页面信息标记与提取方法  

### 1. Beautiful Soup 库的理解  
   
   ![](/assets/image_Spider/beautiful%20soup%20库的理解.png)

### 2. Beautiful Soup 库解析器  
   
   ![](/assets/image_Spider/beautiful%20soup%20库解析器.png)

### 3. Beautiful Soup 类的基本元素  
   
   ![](/assets/image_Spider/beautiful%20soup%20库类的基本元素.png)

### 4. 遍历  
   * 标签树的下行遍历：  
     ![](/assets/image_Spider/标签树的下行遍历.png)

   * 标签树的上行遍历：
     ![](/assets/image_Spider/标签树的上行遍历.png)

   * 标签树的平行遍历：
     ![](/assets/image_Spider/标签树的平行遍历.png)
   
   * **注意：节点不一定都是 Tag 类型， 也可能是 NavigbleString 类型，所以在遍历的时候要注意节点类型判断**

### 5. 3种信息标记形式  

   * XML  
     用尖括号表示。扩展性好，但繁琐

     ![](/assets/image_Spider/XML实例.png)
   * JSON  
     有类型键值对。信息有类型，适合程序处理(js)，比XML简洁

     ![](/assets/image_Spider/JSON实例.png)
   * YAML  
     无类型键值对。文本信息比例最高，可读性好

     ![](/assets/image_Spider/YAML实例.png)

### 6. 信息提取的一般方法
   
   * 方法一：完整解析信息的标记形式，再提取关键信息
   * 方法二：无视标记形式，直接搜索关键信息
   * 融合方法，结合形式解析与搜索的方法，提取关键信息  
     <>.find_all() 方法   
 

     ![](/assets/image_Spider/find_all.png)

     ![](/assets/image_Spider/find_all2.png)  

     1. ```soup.find_all('p', 'coures')```： 查找含有'coures' 字符串的 p 标签，返回标签列表；  
     2. ```soup.find_all(id='link1')```： 查找属性 id='link1' 的标签，返回标签列表；  
     3. ```soup.find_all(string ='python') ```： 查找字符串区域是'python'字符串的标签，返回标签列表。查找含有'python'字符串的标签，用正则表达式：
         ```python
         import re
         soup.find_all(string = re.compile('python')) 
         ``` 
   
     <>.find_all() 扩展方法  
     ![](/assets/image_Spider/find_all扩展方法.png)


### 7. 中国大学排名爬虫  
   中国大学排名爬虫：
   ```python
   # 获取中国大学排名的定向爬虫

   import requests
   from bs4 import BeautifulSoup
   import bs4


   def getHTMLText(url):
      """
      获取HTML网页信息
      """
      try:
         r = requests.get(url, timeout=10)
         r.raise_for_status()
         r.encoding = r.apparent_encoding
         return r.text
      except:
         return ''


   def fillUnivList(ulist: list, html):
      """
      解析网页信息,得到大学排名数据并存入列表
      """
      soup = BeautifulSoup(html, 'html.parser')
      for tr in soup.find('tbody').children:
         if isinstance(tr, bs4.element.Tag):
               tds = tr('td')
               ulist.append([tds[0].string.strip(), tds[1].a.string.strip(), tds[4].string.strip()])


   def printUnivList(ulist, num):
      """
      指定格式输出
      """
      print('{0:^10}\t{1:{3}^10}\t{2:^10}'.format('排名', '学校名称', '总分', chr(12288)))
      for i in range(num):
         item = ulist[i]
         print('{0:^10}\t{1:{3}^10}\t{2:^10}'.format(item[0], item[1], item[2], chr(12288)))


   def main():
      url = 'https://www.shanghairanking.cn/rankings/bcur/2021'
      univInfo = []
      html = getHTMLText(url)
      if html == '':
         print('爬取失败')
      else:
         fillUnivList(univInfo, html)
         printUnivList(univInfo, 20)


   main()
   ```
   ```format()```方法相关约定：  
   ![](/assets/image_Spider/format约定.png)

   中文空格：```chr(12288)```

## 四、正则表达式

### 1. 正则表达式常用操作符：  
   
   ![](/assets/image_Spider/正则表达式常用操作符.png)

   ![](/assets/image_Spider/正则表达式常用操作符2.png)

### 2. 正则表达式实例：
   
   ![](/assets/image_Spider/正则表达式实例.png)
   ![](/assets/image_Spider/正则表达式实例2.png)

### 3. re 库主要的功能函数
   ![](/assets/image_Spider/re库功能函数.png)
   
   1. re.search(pattern, string, flags=0)：  
      * pattern: 正则表达式的字符串或原生字符串表示
      * string: 待匹配字符串
      * flags: 正则表达式使用时的控制标记  
         ![](/assets/image_Spider/re表达式控制标记.png)  

      ```python
      match = re.search(r'[1-9]\d{5}', '1008631ds45643345g')
         if match:
            print(match.group(0)) 

      '100863'
      ```
   2. re.match(pattern, string, flags=0)： 
      
      ```python
      # 匹配邮政编码
      match = re.match(r'[1-9]\d{5}', '1008631dsg')
         if match:
            print(match.group(0))


      '100863'
      ```
   3. re.finall(pattern, string, flags=0):
      
      ```python
      ls = re.findall(r'[1-9]\d{5}', '1008631ds45643345g')
         print(ls)
      

      ['100863', '456433']
      ```
   4. re.split(pattern, string, maxsplit=0, flags=0)
      * maxsplit: 最大分割数，剩余部分作为最后一个元素输出
      ```python
      >>> import re
      >>> re.split(r'[1-9]\d{5}', 'bit1008745 tsu100867634')
      ['bit', '5 tsu', '634']

      >>> re.split(r'[1-9]\d{5}', 'bit1008745 tsu100867634', maxsplit=1)
      ['bit', '5 tsu100867634']
      ```
   
   5. re.finditer(pattern, string, flags=0):
      ```python
      >>> for m in re.finditer(r'[1-9]\d{5}', 'bit1008745 tsu100867634'):
	         if m:
		         print(m.group(0))

		
         100874
         100867
      ```
   
   6. re.sub(pattern, repl, string, count=0, flags=0):
      * repl: 替换匹配字符串的字符串
      * count: 匹配的最大替换次数
      ```python
      >>> re.sub(r'[1-9]\d{5}', ':zipcode', 'bit1008745 tsu100867634')
      'bit:zipcode5 tsu:zipcode634' 
      ```

### 4. re 库的另一种等价用法  
   ![](/assets/image_Spider/re库的另一种等价用法.png)  

   * regex = re.compile(pattern, flags=0):  
     将正则表达式的字符串形式编译成正则表达式对象
   * 等价方法：
     ![](/assets/image_Spider/re库的另一种等价方法.png) 

### 5. re 库的match 对象 
   * match 对象属性：
     ![](/assets/image_Spider/match对象属性.png) 
   * match 对象方法：
     ![](/assets/image_Spider/match%20对象方法.png)  

     ```python
      >>> res = re.search(r'[1-9]\d{5}', 'bit1008745 tsu100867634')
      >>> res
      <re.Match object; span=(3, 9), match='100874'>
      >>> m = re.search(r'[1-9]\d{5}', 'bit1008745 tsu100867634')
      >>> m.string
      'bit1008745 tsu100867634'
      >>> m.re
      re.compile('[1-9]\\d{5}')
      >>> m.pos
      0
      >>> m.endpos
      23
      >>> m.group(0)
      '100874'
      >>> m.start()
      3
      >>> m.end()
      9
      >>> m.span()
      (3, 9)
     ```

### 6. re库的贪婪匹配和最小匹配  

1. re库默认采用贪婪匹配的方式，即输出匹配的最长的子串：

   ```python
   >>> match = re.search(r'PY.*N', 'PYANBNCNDN')
   >>> match.group(0)
   'PYANBNCNDN'
   ```

2. 最小匹配操作符：
   ![](/assets/image_Spider/最小匹配操作符.png)  

   ```python
   >>> match = re.search(r'PY.*?N', 'PYANBNCNDN')
   >>> match.group(0)
   'PYAN'
   ```

3. 淘宝商品搜索定向爬虫
 
   ```python
   import re
   import requests
   import sys


   # 定向爬虫，爬取商品名称和价格

   def getHTMLText(url):
      headers = {
         'DNT': '1',
         'Referer': 'https://s.taobao.com/',
         'sec-ch-ua': '"Chromium";v="92", " Not A;Brand";v="99", "Google Chrome";v="92"',
         'sec-ch-ua-mobile': '?0',
         # 需要cookie模拟登录
         'cookie': 'cna=V+LUGC92xTsCAX1GOKG7K45v; tracknick=%5Cu4E28%5Cu9A6C%5Cu4E36%5Cu4E91%5Cu4E28; _cc_=V32FPkk%2Fhw%3D%3D; sgcookie=E100Z29xFUONO75f7IiE7hmW2rcxFc4GU9URIOZBXlhdndgX5gqW%2BdioaUKCwvagZB4QvkNAQPtOYZX67v3qCkpcpw%3D%3D; thw=cn; enc=WvP3MDQDek1onAyJv1oKNbuQELc5HuEPiR7Jrr3WJAVunczV5YZu9H14sdNSj8qDLwkaipN5Fzll6iUwS7E9Pw%3D%3D; hng=CN%7Czh-CN%7CCNY%7C156; UM_distinctid=17830838f4533a-0d1bfb4d027bda-5771133-1fa400-17830838f46afb; JSESSIONID=EDCF4B3641F08AF21DBFCB0294386DEA; xlly_s=1; cookie2=1464af1ea1d5b208fc7b261153569fad; t=24cf01d975ce2a0eaedbacd1ac966969; _tb_token_=337736e885b5e; v=0; mt=ci%3D-1_1; tfstk=cNARBwt9_mmk6A7v7LHmAeRkEB5RaUodczs394VvRB-QJxl-NsYwsCF0YY_Q2s3A.; l=eBIhj1z4jRu7d8owBO5CKurza7792Ldf1oVzaNbMiInca6BPa_pa5NCKq04HudtjgtCXuUxPVKlOcR3D8i4dg2HvCbKrCyConxvO.; isg=BI2N06Fy0Eu7vHUNy5ksOBSfnKkHasE8k-nCns8ZfCZFxq54nbsdDJ4UMFqgAtn0',
         'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.159 Safari/537.36'
      }
      try:
         r = requests.get(url, headers=headers)
         r.raise_for_status()
         if r.status_code == 200:
               print('读取商品页面成功')
         r.encoding = r.apparent_encoding
         return r.text
      except:
         print('读取失败')


   def parseTEXT(glist: list, html):
      try:
         name = re.findall(r'"raw_title":".*?"', html)  # 匹配商品名称字符串如："raw_title":"谷歌/Google Pixel 4a/Pixel 4A/Pixel 4a手机谷歌手机现货"
         pric = re.findall(r'"view_price":"[\d.]*"', html)  # 匹配商品价格字符串如："view_price":"2450.00"
         name = [eval(i.split(':')[-1]) for i in name]
         pric = [eval(i.split(':')[-1]) for i in pric]
         for item in zip(name, pric):
               glist.append(item)
         print('解析商品数据成功')
      except:
         print('解析商品数据出错')


   def printGoodsList(glist: list):
      form = '{:4}\t{:4}\t\t{:16}'
      print(form.format('序号', '价格', '商品名称'))
      count = 1
      for i in glist:
         print(form.format(count, i[1], i[0]))
         count += 1
      print('打印完毕')


   def main():
      try:
         goods = input('请输入你要查找的商品：')  # 需要爬取的商品
         page = int(input('请输入要展示多少个页面的商品：'))    # 爬取页面深度
      except:
         print('输入错误')
         sys.exit()

      glist = []

      starturl = 'https://s.taobao.com/search?q=' + goods

      for i in range(page):
         url = starturl + '&s=' + str(i*44)
         print('\n获取第 {} 页商品:'.format(i+1))

         print('正在读取商品页面...')
         html = getHTMLText(url)

         print('正在解析商品数据...')
         parseTEXT(glist, html)

      print('\n打印商品信息：\n')
      printGoodsList(glist)

   main()


   ```

   **去除字符串的双引号： eval(string)** 

4. 股票数据定向爬虫
   ```python
   import re
   import traceback

   import requests
   from bs4 import BeautifulSoup

   # 定向爬取股票数据

   def getHTMLText(url):
      headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.159 Safari/537.36'}
      try:
         r = requests.get(url, headers=headers)
         r.raise_for_status()
         r.encoding = r.apparent_encoding
         return r.text
      except:
         print('读取失败')


   def getStockList(stocklst: list, url):
      html = getHTMLText(url)
      soup = BeautifulSoup(html, 'html.parser')
      div = soup.find('div', class_='quotebody')  # 注意 'class' 是python 的关键字,直接用会报错，需改用 'class_'
      a = div.find_all('a')
      for i in a:
         # 注意这里的 a 标签不仅有记录股票代码的标签，还有其他类型的 a 标签，
         # 因此不是所有的 a 标签都符合正则表达式的方式，会出现错误和异常，
         # 用 try...continue 可以过滤掉这些异常
         try:
               href = i.attrs['href']
               stocklst.append(re.findall(r'[s][hz]\d{6}', href)[0])
         except:
               continue


   def getStockInfo(stocklst:list, stockurl, fpath):
      count = 0   # 进度计数
      for i in stocklst:
         url = stockurl + i[-6:] + '/index.shtml'
         html = getHTMLText(url)

         try:
               if html == '':
                  continue
               infoDict = {}
               soup = BeautifulSoup(html, 'html.parser')
               li = soup.find('li', class_='name')
               name = li.find('a').text
               infoDict.update({'股票名称': name.strip(), '股票编号': i})
               with open(fpath, 'a', encoding='utf-8') as f:
                  f.write(str(infoDict) + '\n')
                  count += 1
                  print('\r当前进度：{:.2f}%'.format(count*100/len(stocklst)), end='')  # 动态进度条显示

         except:
               traceback.print_exc()
               count += 1
               print('\r当前进度：{:.2f}%'.format(count * 100 / len(stocklst)), end='')
               continue


   def main():
      stock_list_url = 'http://quote.eastmoney.com/stocklist.Html'    # 东方财富
      stock_info_url = 'https://q.stock.sohu.com/cn/'     # 搜狐财经
      output_file = 'E://SouhuStockInfo.txt'
      stocklst = []
      getStockList(stocklst, stock_list_url)
      getStockInfo(stocklst, stock_info_url, output_file)

   main()
   ```