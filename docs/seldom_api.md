## seledom API

### 查找元素

seldom 提供了8中定位方式，与Selenium保持一致。

* id_
* name
* class_name
* tag
* link_text
* partial_link_text
* css
* xpath

__使用方式__

```python
self.type(id_="kw", text="seldom")
self.type(name="wd", text="seldom")
self.type(class_name="s_ipt", text="seldom")
self.type(tag="input", text="seldom")
self.type(xpath="//input[@id='kw']", text="seldom")
self.type(css="#kw", text="seldom")
self.click(link_text="hao123")
self.click(partial_link_text="hao")
```

__帮助信息__

* [CSS选择器](https://www.w3school.com.cn/cssref/css_selectors.asp)
* [xpath语法](https://www.w3school.com.cn/xpath/xpath_syntax.asp)


__使用下标__

有时候无法通过一种定位找到单个元素，那么可以通过`index`指定一组元素中的第几个。

```py
self.type(tag="input", index=7, text="seldom")
```

通过`tag="input"`匹配出一组元素， `index=7` 指定这一组元素中的第8个，`index`默认下标为`0`。


### fixture

有时自动化测试用例的运行需要一些前置&后置步骤，seldom提供了相应的方法。

__start & end__

针对每条用例的fixture，可以放到`start()/end()`方法中。

```python
import seldom


class TestCase(seldom.TestCase):

    def start(self):
        print("一条测试用例开始")

    def end(self):
        print("一条测试结果")

    def test_search_seldom(self):
        self.open("https://www.baidu.com")
        self.type_enter(id_="kw", text="seldom")

    def test_search_poium(self):
        self.open("https://www.baidu.com")
        self.type_enter(id_="kw", text="poium")

```

__start_class & end_class__

针对每个测试类的fixture，可以放到`start_class()/end_class()`方法中。

```python
import seldom


class TestCase(seldom.TestCase):

    def start_class(self):
        print("测试类开始执行")

    def end_class(self):
        print("测试类结束执行")

    def test_search_seldom(self):
        self.open("https://www.baidu.com")
        self.type_enter(id_="kw", text="seldom", clear=True)

    def test_search_poium(self):
        self.open("https://www.baidu.com")
        self.type_enter(id_="kw", text="poium", clear=True)

```

> 警告：不要把用例的操作步骤写到fixture方法中! 因为它不属于某条用例的一部分，一旦里面的操作步骤运行失败，测试报告都不会生成。

### 断言

seldom 提供了一组针对Web页面的断言方法。

__使用方法__

```python
# 断言标题是否等于"title"
self.assertTitle("title")

# 断言标题是否包含"title"
self.assertInTitle("title")

# 断言URL是否等于
self.assertUrl("url")

# 断言URL是否包含
self.assertInUrl("url")

# 断言页面包含“text”
self.assertText("text")

# 断言页面不包含“text”
self.assertNotText("text")


# 断言警告是否存在"text" 提示信息
self.assertAlertText("text")

# 断言元素是否存在
self.assertElement(css="#kw")

# 断言元素是否不存在
self.assertNotElement(css="#kwasdfasdfa")
```

### 跳过测试用例

seldom 提供了跳过用例的装饰用于跳过暂时不执行的用例。

__装饰器__

* skip: 无条件地跳过一个测试。
* skip_if： 如果条件为真，则跳过测试。
* skip_unless: 跳过一个测试，除非条件为真。
* expected_failure: 预期测试用例会失败。

__使用方法__

```python
import seldom

@seldom.skip()  # 跳过测试类
class YouTest(seldom.TestCase):

    @seldom.skip()  # 跳过测试用例
    def test_case(self):
        # ...


if __name__ == '__main__':
    seldom.main()
```


### WebDriver API

seldom简化了selenium中的API，使操作Web页面更加简单。

大部分API都由`WebDriver`类提供：

```python
import seldom

class TestCase(seldom.TestCase):

    def test_seldom_api(self):
        # Accept warning box.
        self.accept_alert()
        
        # Adds a cookie to your current session.
        self.add_cookie({'name' : 'foo', 'value' : 'bar'})
        
        # Adds a cookie to your current session.
        cookie_list = [
            {'name' : 'foo', 'value' : 'bar'},
            {'name' : 'foo', 'value' : 'bar'}
        ]
        self.add_cookie(cookie_list)
        
        
        # Clear the contents of the input box.
        self.clear(css="#el")
        
        # It can click any text / image can be clicked
        # Connection, check box, radio buttons, and even drop-down box etc..
        self.click(css="#el")
        
        # Mouse over the element.
        self.move_to_element(css="#el")
        
        # Click the element by the link text
        self.click_text("新闻")
        
        # Simulates the user clicking the "close" button in the titlebar of a popup window or tab.
        self.close()
        
        # Delete all cookies in the scope of the session.
        self.delete_all_cookies()
        
        # Deletes a single cookie with the given name.
        self.delete_cookie('my_cookie')
        
        # Dismisses the alert available.
        self.dismiss_alert()
        
        # Double click element.
        self.double_click(css="#el")
        
        # Execute JavaScript scripts.
        self.execute_script("window.scrollTo(200,1000);")
        
        # Setting width and height of window scroll bar.
        self.window_scroll(width=300, height=500)
        
        # Setting width and height of element scroll bar.
        self.element_scroll(css=".class", width=300, height=500)
        
        # open url.
        self.open("https://www.baidu.com")
        
        # Gets the text of the Alert.
        self.get_alert_text()
        
        # Gets the value of an element attribute.
        self.get_attribute(css="#el", attribute="type")
        
        # Returns information of cookie with ``name`` as an object.
        self.get_cookie()
        
        # Returns a set of dictionaries, corresponding to cookies visible in the current session.
        self.get_cookies()
        
        # Gets the element to display,The return result is true or false.
        self.get_display(css="#el")
        
        # Get a set of elements
        self.get_element(css="#el", index=0)
        
        # Get element text information.
        self.get_text(css="#el")
        
        # Get window title.
        self.get_title()
        
        # Get the URL address of the current page.
        self.get_url()
        
        # Set browser window maximized.
        self.max_window()
        
        # Mouse over the element.
        self.move_to_element(css="#el")
        
        # open url.
        self.open("https://www.baidu.com")
        
        # Quit the driver and close all the windows.
        self.quit()
        
        # Refresh the current page.
        self.refresh()
        
        # Right click element.
        self.right_click(css="#el")
        
        # Saves a screenshots of the current window to a PNG image file.
        self.screenshots('/Screenshots/foo.png')
        
        """
        Constructor. A check is made that the given element is, indeed, a SELECT tag. If it is not,
        then an UnexpectedTagNameException is thrown.
        <select name="NR" id="nr">
            <option value="10" selected="">每页显示10条</option>
            <option value="20">每页显示20条</option>
            <option value="50">每页显示50条</option>
        </select>
        """
        self.select(css="#nr", value='20')
        self.select(css="#nr", text='每页显示20条')
        self.select(css="#nr", index=2)
        
        # Set browser window wide and high.
        self.set_window(100, 200)
        
        # Submit the specified form.
        self.submit(css="#el")
        
        # Switch to the specified frame.
        self.switch_to_frame(css="#el")
        
        # Returns the current form machine form at the next higher level.
        # Corresponding relationship with switch_to_frame () method.
        self.switch_to_frame_out()
        
        
        # Switches focus to the specified window.
        # This switches to the new windows/tab (0 is the first one)
        self.switch_to_window(1)
        
        # Operation input box.
        self.type(css="#el", text="selenium")
        
        
        # Implicitly wait.All elements on the page.
        self.wait(10)
        
        # Setting width and height of window scroll bar.
        self.window_scroll(width=300, height=500)

```

### 键盘操作

有时候我们需要用到键盘操作，比如`Enter`，`Backspace`，`TAB`，或者`ctrl/command + a`、`ctrl/command + c`组合键操作，seldom提供了一组键盘操作。

__使用方法__

```py
import seldom


class Test(seldom.TestCase):

    def test_key(self):
        self.open("https://www.baidu.com")

        # 输入 seldomm
        self.Keys(css="#kw").input("seldomm")

        # 删除多输入的一个m
        self.Keys(id_="kw").backspace()

        # 输入“教程”
        self.Keys(id_="kw").input("教程")

        # ctrl+a 全选输入框内容
        self.Keys(id_="kw").select_all()

        # ctrl+x 剪切输入框内容
        self.Keys(id_="kw").cut()

        # ctrl+v 粘贴内容到输入框
        self.Keys(id_="kw").paste()

        # 通过回车键来代替单击操作
        self.Keys(id_="kw").enter()


if __name__ == '__main__':
    seldom.main(browser="firefox", debug=True)

```
