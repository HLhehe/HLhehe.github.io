---
title: pytest单元测试框架（进阶篇）
date: 2023-11-27 17:21:21
tags: ['测试',pytest]
---
## 测试夹具

​		1）什么是夹具？

​		在自动化测试过程中，为测试用例提前准备的一个运行环境，这个测试环境被称为测试夹具。测试夹具的本质是一个函数，在测试用例方法执行之前的称为前置条件，测试用例方法执行之后的称为后置条件。

<!-- more -->

​		2）为什么需要夹具

​		比如在进行web ui测试时，需要打开浏览器（前置条件），关闭浏览器（后置条件）；比如在进行接口自动化测试时，接口关联的参数值写入yaml文件，测试之前需要清空yaml文件（前置条件）；比如测试时需要记录日志，日志对象的生成（前置条件），日志对象的销毁（后置条件）。

​		2）常用的pytest夹具

- setup/teardown，setup_class/teardown_class
- @pytest.fixture()
- conftest.py和@pytest.fixture()结合

### setup/teardown，setup_class/teardown_class

```python
import pytest

class TestName():

    age=19
    def testName(self):
        print("hello,pytest")

    @pytest.mark.run(order=99)
   # @pytest.mark.skipif(age>18,reason="年龄已经大于18，不用测啦")
    def testAge(self):
        print("myage")
        
   # @pytest.mark.skip(reason="不想测试这个啦")
    def testAddr(self):
        print("myaddr")

    def setup(self):
        print("开始进行测试用例---作用范围为单个方法")

    def teardown(self):
        print("测试用例结束---作用范围单个方法")

    def setup_class(self):
        print("开始进行测试用例，对TestName类")

    def teardown_class(self):
        print("结束对TestName类的测试")
        
  
'''输出
testcase/test_01.py:: TestName : :testAge开始进行测试用例，对TestName类
开始进行测试用例---作用范围为单个方法
myage
PASSED测试用例结束---作用范围单个方法

testcase/test_01.py : :TestName : :testName 开始进行测试用例---作用范围为单个方法
hello , pytest
PASSED测试用例结束---作用范围单个方法

testcase/test_01.py : :TestName : :testAddr开始进行测试用例---作用范围为单个方法
myaddr
PASSED测试用例结束---作用范围单个方法
结束对TestName类的测试
'''
#不建议使用此方法，抛出了警告信息（已过时），且此方法针对所有的测试用例都必须执行该方法，不能选择一部分执行。
#@pytest.fixture()装饰器模式可以对部分方法的前后置方法
```

### @pytest.fixture()

```python
#上代码
import pytest


@pytest.fixture(scope='function', params=['成龙', '甄子丹', '菜10'])
#这参数有什么作用呢？引出后面的数据驱动DDT
def my_fixture(request):
    print('前置函数')
    yield request.param
    print('后置函数')


class TestPytest:
    def test_Pytest(self):
        print("test01")

    def test_02(self,my_fixture):
        print("test02")
        #print(my_fixture)  //返回成龙，甄子丹，菜10

 """输出
test_Pytest.py::TestPytest::test_Pytest test01 PASSED test_Pytest.py::TestPytest::test_02[成龙] 前置函数
test02 PASSED后置函数

test_Pytest.py::TestPytest::test_02[甄子丹] 前置函数
test02 PASSED后置函数

test_Pytest.py::TestPytest::test_02[菜10] 前置函数
test02 PASSED后置函数
"""
```

```python
@pytest.fixture(scope="",params="",autouse="",ids="",name="")
"""
(1)scope表示的是被@pytest.fixture标记的方法的作用域。function(默认)，class，module，package/session.
		session 会话级别：每个session只运行一次，session级别的fixture需要定义到conftest.py中

		module 模块级别：模块里所有的用例执行前执行一次module级别的fixture，即单个py文件执行一次模块级别

		class 类级别 ：每个类执行前都会执行一次class级别的fixture

		function ：此模式是默认的模式，函数级别的，每个测试用例执行前都会执行一次function级别的fixture
(2)params：参数化（支持，列表[]，元祖()，字典列表[{},{},{}]，字典元祖({},{},{})，循环执行，每次取下一个
(3)autouse=True：自动使用，默认False
(4)ids：当使用params参数化时，给每一个值设置一个变量名。意义不大。
(5)name：给表示的是被@pytest.fixture标记的方法取一个别名。意义不大
"""
"""
yield前面是前置函数，yield后面是后置函数，且yield能将后面的结果return给对应的用例函数
"""
"""
问题：使用时，每个类或者模块里都需要定义对应的函数，才能被调用执行。代码复用性不强
如何在一个地方定义夹具函数，所有的用例函数都能调用到呢？
有请我们的conftest.py文件
"""
```

### conftest.py结合@pytest.fixture()

​		通过使用conftest.py文件和@pytest.fixture结合，实现全局的夹具配置

```py
"""
conftest.py文件是单独存放的一个夹具配置文件，名称是不能更改。
可以在不同的py文件中使用同一个fixture函数。
原则上conftest.py需要和运行的用例放到统一层(一般放在根目录下即可）。并且不需要做任何的imprt导入的操作。
写法与@pytest.fiuture无异，只不过将定义的函数都放在conftest.py文件中
"""
```

### 总结

setup/teardown，setup_class/teardown_class 它是作用于所有用例或者所有的类 

@pytest.fixtrue() 它的作用是既可以部分也可以全部前后置。 

conftest.py和@pytest.fixtrue()结合使用，作用于全局的前后置。

## allure报告

​		1）下载

​		[Releases · allure-framework/allure2 (github.com)](https://github.com/allure-framework/allure2/releases)

​		2）配置bin

​		找到安装的allure文件夹的bin路径，将其加入path中即可。

​		3）使用

```bash
pytest [测试文件] --alluredir=./result #--alluredir用于指定存储测试结果的路径
allure serve ./result  #生成页面报告
```

## 数据驱动DDT

​		DDT即data-driver test，数据驱动测试。 以数据来驱动整个测试用例的执行（即测试数据决定测试结果）,核心使用@pytest.mark.parametrize

```python
import pytest
import yaml

class Test_Demo():
    #前面可以为String，list，元组，逗号分隔
    # @pytest.mark.parametrize("a, b, result", [(1, 1, 2), (2, 8, 10)])
    # @pytest.mark.parametrize(("a, b, result"), data)
    @pytest.mark.parametrize(["a","b","result"],yaml.safe_load(open("./data.yaml")))
    def test_case1(self, a, b, result):
        print("\n开始执行测试用例1")
        assert a + b == result
```







