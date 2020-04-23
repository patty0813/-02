
2.2 学习xpath
2.2.1 学习目标：
学习xpath，使用lxml+xpath提取内容。

使用xpath提取丁香园论坛的回复内容。

抓取丁香园网页：http://www.dxy.cn/bbs/thread/626626#626626 。

2.2.2 Xpath常用的路径表达式：
XPath即为XML路径语言（XML Path Language），它是一种用来确定XML文档中某部分位置的语言。
在XPath中，有七种类型的节点：元素、属性、文本、命名空间、处理指令、注释以及文档（根）节点。
XML文档是被作为节点树来对待的。
XPath使用路径表达式在XML文档中选取节点。节点是通过沿着路径选取的。下面列出了最常用的路径表达式：

nodename 选取此节点的所有子节点。
/ 从根节点选取。
// 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。
. 选取当前节点。
.. 选取当前节点的父节点。
@ 选取属性。
/text() 提取标签下面的文本内容

如：
/标签名 逐层提取
/标签名 提取所有名为<>的标签
//标签名[@属性=“属性值”] 提取包含属性为属性值的标签
@属性名 代表取某个属性名的属性值
详细学习：https://www.cnblogs.com/gaojun/archive/2012/08/11/2633908.html

2.2.3 使用lxml解析
导入库：from lxml import etree

lxml将html文本转成xml对象

tree = etree.HTML(html)
用户名称：tree.xpath(’//div[@class=“auth”]/a/text()’)

回复内容：tree.xpath(’//td[@class=“postbody”]’) 因为回复内容中有换行等标签，所以需要用string()来获取数据。

string()的详细见链接：https://www.cnblogs.com/CYHISTW/p/12312570.html
Xpath中text()，string()，data()的区别如下：

text()仅仅返回所指元素的文本内容。
string()函数会得到所指元素的所有节点文本内容，这些文本讲会被拼接成一个字符串。
data()大多数时候，data()函数和string()函数通用，而且不建议经常使用data()函数，有数据表明，该函数会影响XPath的性能。
2.2.4 实战：爬取丁香园-用户名和回复内容
爬取思路：
获取url的html
lxml解析html
利用Xpath表达式获取user和content
保存爬取的内容
In [1]:
# 导入库
from lxml import etree
import requests

url = "http://www.dxy.cn/bbs/thread/626626#626626"
1. 获取url的html
In [2]:
req = requests.get(url)
html = req.text
# html
2. lxml解析html
In [3]:
tree = etree.HTML(html) 
tree
Out[3]:
<Element html at 0x2313e5ac188>
3. 利用Xpath表达式获取user和content（完成xpath的语句）
In [4]:
user = tree.xpath('')
# print(user)
content = tree.xpath('')
4. 保存爬取的内容
In [11]:
results = []
for i in range(0, len(user)):
    # print(user[i].strip()+":"+content[i].xpath('string(.)').strip())
    # print("*"*80)
    # 因为回复内容中有换行等标签，所以需要用string()来获取数据
    results.append(user[i].strip() + ":  " + content[i].xpath('string(.)').strip())
In [13]:
# 打印爬取的结果
for i,result in zip(range(0, len(user)),results):
    print("user"+ str(i+1) + "-" + result)
    print("*"*100)
user1-楼医生:  我遇到一个“怪”病人，向大家请教。她，42岁。反复惊吓后晕厥30余年。每次受响声惊吓后发生跌倒，短暂意识丧失。无逆行性遗忘，无抽搐，无口吐白沫，无大小便失禁。多次跌倒致外伤。婴儿时有惊厥史。入院查体无殊。ECG、24小时动态心电图无殊；头颅MRI示小软化灶；脑电图无殊。入院后有数次类似发作。请问该患者该做何诊断，还需做什么检查，治疗方案怎样？
****************************************************************************************************
user2-lion000:  从发作的症状上比较符合血管迷走神经性晕厥，直立倾斜试验能协助诊断。在行直立倾斜实验前应该做常规的体格检查、ECG、UCG、holter和X-ray胸片除外器质性心脏病。贴一篇“口服氨酰心安和依那普利治疗血管迷走性晕厥的疗效观察”作者：林文华 任自文 丁燕生http://www.ccheart.com.cn/ccheart_site/Templates/jieru/200011/1-1.htm
****************************************************************************************************
user3-xghrh:  同意lion000版主的观点：如果此患者随着年龄的增长，其发作频率逐渐减少且更加支持，不知此患者有无这一特点。入院后的HOLTER及血压监测对此患者只能是一种安慰性的检查，因在这些检查过程中患者发病的机会不是太大，当然不排除正好发作的情况。对此患者应常规作直立倾斜试验，如果没有诱发出，再考虑有无可能是其他原因所致的意识障碍，如室性心动过速等，但这需要电生理尤其是心腔内电生理的检查，毕竟是有一种创伤性方法。因在外地，下面一篇文章可能对您有助，请您自己查找一下。心理应激事件诱发血管迷走性晕厥1例 ，杨峻青、吴沃栋、张瑞云，中国神经精神疾病杂志， 2002 Vol.28 No.2
****************************************************************************************************
user4-keys:  该例不排除精神因素导致的，因为每次均在受惊吓后出现。当然，在作出此诊断前，应完善相关检查，如头颅MIR(MRA),直立倾斜试验等。
****************************************************************************************************
In [ ]:



