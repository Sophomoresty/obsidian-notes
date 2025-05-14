## 1.浏览器基本操作
需要导入的包
``` python
from selenium import webdriver  # 浏览器对象
from selenium.webdriver.chrome.service import Service  # 指定驱动器位置
from selenium.webdriver.chrome.options import Options  # 指定参数
```
需要实例化两个Service, Options对象, 这两个对象负责给浏览器对象传参

``` python
service = Service(executable_path=r'E:\ChromeDriver\chromedriver.exe')  # 驱动位置

opt = Options()
opt.add_argument('--disable-blink-features=AutomationControlled')  # 隐藏selenium的痕迹

browser = webdriver.Chrome(service=service, options=opt)  # 实例化浏览器对象
```

1. 打开浏览器
> browser.get(url)
2. 设置窗口大小
> browser.set_window_size(长的像素,宽的像素) 
3. 设置打开浏览器位置
> browser.set_window_postion(x,y)  # x, y是相对于屏幕左上角的位置, 单位像素
4. 关闭浏览器
> browser.close()  # 关闭当前网页
> browser.quit()  # 关闭浏览器
5. 前进
> browser.forward()
6. 后退
> browser.back()
7. 刷新
> browser.refresh()
8. 获取网页代码
> browser.page_source


## 2.定位网页页面元素
```
from selenium.webdriver.common.by import By  # selenium提供的定位包
from selenium.webdriver.common.keys import Keys  # 提供键盘操作
```
### 定位网页页面元素
1. 根据ID定位
> element = browser.find_element(By.ID, 'ID')
2. 根据class定位
> element = browser.find_element(By.CLASS_NAME,'CLASS_NAME')
3. 根据标签名定位 不太清楚这个该怎么用 需查文档
> element = browser.find_element(By.TAG_NAME,'TAG_NAME')
4. 根据CSS选择器定位
> element = browser.find_element(By.CSS-SELECTOR,'CSS-SELECTOR')
5. 根据name定位 不太清楚这个该怎么用 需查文档
> element = browser.find_element(By.NAME, 'NAME')
6. 根据XPath定位
> element = browser.find_element(By.XPATH, 'XPATH')
7. 根据链接文本定位 不太清楚这个该怎么用 需查文档
> element = browser.find_element(By.LINK_TEXT,'LINK_TEXT')

### 输入操作和点击
> element.send_keys('输入的内容',Keys.操作)
> element.click()

## 3.设置元素等待及加载策略

### 页面加载策略:
1. normal(默认): 完整地加载:把get地址的页面及所有静态资源都下载完(如css, 图片, js等)
2. eager: 等待初始HTML文档完全加载和解析,并放弃样式表, 图表盒子框架的加载
3. none:仅等待初始页面的下载. 从现象来看就是打开浏览器输入网址, 然后不管了

```python
opt.page_load_strategy = 'eager'  # 设置页面加载策略
```

### 设置webdriver等待

很多页面都使用ajax技术, 页面的元素不是同时被家长出来的, 为了防止定位这些尚在加载的元素报错, 可以设置元素等来增加脚本的稳定性. webdriver中的等待分为显示等待和隐式等待.

#### 隐式等待:
设置一个超时时间, 如果超出这个时间, 指定元素还没有被加载出来, 就会抛出 NoSuchElementException 异常.
除了抛出的异常不同外, 还有一点, 隐式等待是全局性的, 即运行过程中, 如果元素可以定位到, 它不会影响代码运行, 但如果定位不到, 则它会以轮询的方式不断地访问元素直到元素被找到, 若超过指定时间, 则抛出异常.

```python
# browser初始化完成后
browser.implicitly_wait(数字) 数字是等待的时间, 单位s 
```

```python
# 通过by的找到的元素可以通过get_attribute('')找到属性值
```

#### 显示等待
设置一个超时时间, 每隔一段时间就去检测一次该元素是否存在, 如果存在则执行后续内容, 如果超过最大时间(超时时间)则抛出超时异常(TimeoutException). 显示等待需要使用WebDriverWait, 同时配合until或not until

导包
```python
from selenium.webdriver.support.ui import WebDriverWait  # 显式等待
from selenium.webdriver.support import expected_conditions as EC  # 增加判断条件
```

```python 
WebDriverWait(browser,5).until(EC.判断条件)
```

```python
locator = (By.XPATH,'xpath内容')
try:
	WebDriverWait(browser,5).until(Ec.presence_of_element_located(locator))
except:
	print('未找到指定元素')
	browser.close()
	exit()  # 退出程序    
```


<img src="E:\Obsidian Vault\assets\image-20250113192336862.png" alt="image-20250113192336862" style="zoom:50%;" />

## 4.浏览器窗口切换
在当我们点击页面按钮时，它一般会打开一个新的标签页，但实际上代码并没有切换到最新页面中，这时你如果要定位新页面的标签就会发现定位不到，这时就需要将实际窗口切换到最新打开的那个窗口。

我们先获取当前各个窗口的句柄，这些信息的保存顺序是按照时间来的，最新打开的窗口放在数组的末尾，这时我们就可以定位到最新打开的那个窗口了。 
driver.switch_to.window(driver.window_handles[-1])


切换窗口
```python
# 切换窗口
browser.switch_to.window(browser.window_handles[-1])
# 打开一个新的页面并切换至新页面, 一般这个方法后面要跟browser.get(url)
browser.switch_to.new_window('tab')
# 打开一个新的窗口并切换至新窗口
browser.switch_to.new_window('window')
```

## 5.表单切换

很多页面也会用带frame/iframe表单嵌套，对于这种内嵌的页面selenium是无法直接定位的，需要使用switch_to.frameO方法将当前操作的对象切换成frame/iframe 内嵌的页面。

switch_to.frame()默认可以用的id或name属性直接定位，但如果iframe没有id或name，这时就需要使用xpath进行定位。
browser.switch_to.default_content 可以切换到默认窗口

## 6.动作链

什么是动作链

用selenium做爬虫，有时候会遇到需要模拟鼠标和键盘操作才能进行的情况，比如单击、双击、点击鼠标右键、拖拽、滚动等等。而selenium给我们提供了一个类来处理这类事件:ActionChainso

常用方法

- move_to_element()
将鼠标移动到指定element，参数为标签。
- move_by_offset(xoffset,yoffset)
将鼠标移动到与当前鼠标位置的偏移处.参数为X轴Y轴上移动的距离。距离单位为像素，可以通过截图的方式来把握距离.
- click()
点击一个标签
- scroll_to_element(iframe)
鼠标滚轮滚动到某个元素
- scroll_by_amount(O,delta_y)
鼠标滚轮安装偏移量滚动
- perform()
执行所有存储的操作。因为行为链是一系列的动作，上边的命令不会写一个执行一个，执行要通过performo命令来全部执行。
- context_click(element)
右键点击一个标签
- click_andhold(element)
点击且不松开鼠标
- double_click(element)
双击。

## 7.防止检测1 - 使用stealth.min.js文件
使用stealth.min.js文件
stealth.min.js文件来源于puppeteer，有开发者给 puppeteer 写了一套插件，叫做puppeteer-extra。其中，就有一个插件叫做puppeteer-extra-plugin-stealth专门用来让 puppeteer隐藏模拟浏览器的指纹特征。
python开发者就需要把其中的隐藏特征的脚本提取出来，做成一个js 文件。然后让 Selenium 或者 Pyppeteer 在打开任意网页之前，先运行一下这个 js 文件里面的内容。

puppeteer-extra-plugin-stealth的作者还写了另外一个工具，叫做extract-stealth evasions。这个东西就是用来生成stealth.min.js文件的。

使用stealth.min.js文件
下载地址：
https://github.com/requireCool/stealth.min.js

``` python
with open(r'E:\ChromeDriver\stealth.min.js', 'r', encoding='utf-8') as f:
    js = f.read()
browser.execute_cdp_cmd("Page.addScriptToEvaluateOnNewDocument", {
    "source": js
})
```

检测selenium的网站
https://bot.sannysoft.com/


## 目前为止经常使用的配置
```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By

opt = Options()
opt.add_argument('--disable-blink-features=AutomationControlled')  # 隐藏隐藏浏览器痕迹
opt.add_experimental_option('excludeSwitches', ['enable-automation'])  # 隐藏自动化浏览器控制的标识
opt.add_experimental_option('detach', True)  # 防止浏览器自动退出
service = Service(r'E:\ChromeDriver\chromedriver.exe')

browser = webdriver.Chrome(service=service, options=opt)
```


## 8.防止检测2 - 使用debugging模式

让selenium连接事先配置好的浏览器。

### 应用场景
爬取需要登录才能获取的内容,比如扫码登录、手机验证码登录等,通过这种方式绕过短期无法解决的验证码的识别; 也可以通过这种方式绕过一些无法自动完成的复杂操作，然后自动再执行后面的爬虫工作.

### 创建并配置浏览器
1. 找到Chrome浏览器的安装路径:
> 比如：C:\Program Files\Google\Chrome\Application
C:\Program Files\Google\Chrome\Application

2. 在命令提示符输入下面命令创建配置一个浏览器：
> chrome.exe --remote-debugging-port='端口' --user-data-dir="安装路径"
chrome.exe --remote-debugging-port='8888' --user-data-dir="E:\Selenium_files\Profiles1"

3. 快捷方式设置参数:
> 在chrome的快捷方式上右击，选择属性，快捷方式的目标栏后面加空格加上下面命令：
> "C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=8888 --user-data-dir="D:\Selenium_files\Profiles1"

"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=8888 --user-data-dir="E:\Selenium_files\Profiles1"

### 使用方法
注意: 使用前要提前打开配置好的浏览器

以下两个参数在此方法中无法使用, 会提示异常
> opt.add_experimental_option('excludeSwitches', ['enable-automation'])  # 隐藏自动化浏览器控制的标识
> opt.add_experimental_option('detach', True)  # 防止浏览器自动退出
```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options

service = Service(r'E:\ChromeDriver\chromedriver.exe')
opt = Options()
opt.add_argument('--disable-blink-features=AutomationControlled')  # 隐藏隐藏浏览器痕迹

# 两种方式设置debugger模式
opt.debugger_address = '127.0.0.1:8888' 
# opt.add_experimental_option("debuggerAddress", "127.0.0.1:8888")


browser = webdriver.Chrome(service=service, options=opt)
url = 'https://www.douban.com/'
browser.get(url)
```

## 防止检测3 - 使用undetected_chromedriver

undetected_chromedriver是Python爱好者基于selenium，专门开发用来可以防止浏览器特征被识别的库，目前能规避大多数检测，并且可以根据浏览器版本自动下载驱动。
但是目前版本更新较慢，随着Chorme浏览器不断更新，各种兼容性问题不断涌现。