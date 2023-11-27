---
title: pytest单元测试框架（基础篇）
date: 2023-11-21 17:21:21
tags: ['测试'，pytest]
---

### 单元测试框架

​		1）什么是单元测试框架

​		单元测试框架是指在软件开发的过程种，针对软件的最小单位（函数、方法）进行正确性测试

<!-- more -->

​		2）常见的单元测试框架

​		基于java的：junit和testng

​		基于python的：unittest和**pytest**

​		3）单元测试流程

​		1.测试发现：从多个文件中找到需要被测试的测试用例

​		2.测试执行：按照一定的顺序执行测试用例，并生成结果

​		3.测试判断：通过assert断言来自动判断结果与预期是否相符

​		4.测试报告：生成这次测试的测试报告，包括耗时、通过率等

## pytest

​		1）简介

​		pytest是python的一种单元测试框架，与python自带的测试框架unittest类似，但比其更易上手，也更加的灵活。因此使用面也广于unittest。

- pytest比unittest更加成熟，同时支持unittest编写的测试用例
- pytest可以和selenium，request，appium等测试框架相结合，实现web ui自动化，web 接口自动化，app ui自动化
- pytest可以实现测试用例的跳过以及失败用例重试
- pytest支持参数化数据驱动方式
- pytest可以和Jenkins持续集成
- pytest具有很多第三方插件，且支持自定义扩展

​				pytest

​				pytest-xdist： 测试用例的分布式执行，即并发执行。可缩短总的用例的执行时间

​				pytest-ordering： 可以指定测试用例的执行顺序，默认是从上到下依次执行

​				pytest-rerunfailures： 用例失败后进行再次执行（多用于ui测试）

​				pytest-html： 生成html格式的测试报告（不够美观）

​				allure-pytest： 生成比上面美观的测试报告，且可进行测试报告的自定义

​		2）安装

​		新建python项目———在本地虚拟终端中执行以下命令即可。（注意：使用以上插件也需要安装到本地环境中，语法类似）

```python
pip install pytest   #安装命令

pytest --version     #检查命令
```

​		3）使用pytest

​		pytest的默认测试用例发现规则

  - 模块名（简单理解为一个py文件）以test_ 开头或 _test结尾

  - 测试类以Test开头，并且不包含init()方法

  - 测试方法以test开头

    4）运行pytest

    ```python
    import pytest
    
    class TestName():
        def testName(self):
            print("hello,pytest")
    
    
    if __name__ == '__main__':
        pytest.main()
    ```

## pytest运行方式

```bash
参数详解:
-s:输出调试信心，包括print打印的信息
-v:显示更详细的信息
-vs:两个连用，效用就是两个相加
-n NUM: 支持多线程或分布式运行测试用例，使用NUM个线程。需要导入pytest-xdist插件
--return NUM:失败用例重跑NUM次，失败用例总共运行NUM+1次
-x:有一个用例失败报错，则停止测试，默认是不停止测试
--maxfail=NUM:出现NUM个失败用例就停止测试
-k String:根据String字符串匹配对应的测试用例
--html 文件路径/文件名：生成html格式的测试报告
```

​		1）主函数方式

```python
#文件夹与python package的区别：python package下包含一个_init_.py文件
# ./  :表示当前文件路径下
# ../  :表示当前文件路径的父路径，即向上一层
pytest.main()    #运行所有的按条件写的测试用例
pytest.main('-vs','./tasecase')   #运行tasecase文件夹下的测试用例
pytest.main('-x','test01.py')    #运行test01模块下的测试用例,且出现错误就停止测试
pytest.mian('-n 2','./tesecase/test01.py::TestName::testName')   #运行testcase文件夹下test01模块里TsetName类里的testName方法,且使用两个线程
```

​		2）命令行模式

```bash
#即在安装pytest的本地虚拟环境中运行
pytest     #运行所有满足条件的测试用例
pytest -vs ./testcase   #运行tasecase文件夹下的测试用例
pytest -x ./testcase/test01.py    #运行test01模块下的测试用例,且出现错误就停止测试
pytest -n 2 ./tesecase/test01.py::TestName::testName    #运行testcase文件夹下test01模块里TsetName类里的testName方法,且使用两个线程
```

​		3）全局配置模式

​		通过读取pytest.ini文件得到对应的配置信息，最后在命令行输入pytest即可运行（名字不可变，只能为pytest.ini）

- 文件位置：放在项目的根目录，即最外层即可
- 编码格式：必须为ANSI格式，可使用notpad++修改格式
- 运行规则：不论是主函数方式还是命令行方式，都会优先读取该配置文件

```yaml
#模板，前面的参数是不可变的，后面根据实际情况填写

[pytest]
addopts = -vs #命令行的参数，用空格分隔
testpaths = ./testcase #测试用例的路径
python_files = test_*.py #模块名的规则，可以更改默认规则
python_classes = Test* #类名的规则
python_functions = test #方法名的规则
markers =
  smoke:冒烟用例          #设置独有的标记
  usermanage:用户管理模块
  productmanage:商品管理模块
```

​		4）改变执行顺序

​		pytest默认的执行顺序是从上到下，改变默认执行顺序，使用mark标记。（需要引入pytest-ordering插件）

```python
import pytest
class TestName():
    
    def testName(self):
        print("hello,pytest")

    @pytest.mark.run(order=99)
    def testAge(self):
        print("myage")

    @pytest.mark.run(order=100)
    def testAddr(self):
        print("myaddr")
        
   #优先执行带注释的用例，且order值越小，优先执行
```

​		5）分组执行

​		在实际的测试场景中，需要对一类的用例进行优先测试，如，冒烟测试、用户管理模块测试、商品管理模块测试等。（还记得上面pytest.ini文件中得makers标记吗？就是用来自定自己的分组的）

```python
@pytest.mark.somke
#在对应的方法上添加注解即可
```

```bash
#执行
pytest -m "smoke" #执行smoke模块测试用例
pytest -m "smoke or usermanage or productmanage"  #执行这三个模块的测试用例
```

​		6）跳过测试用例

```python
import pytest

class TestName():

    age=18
    def testName(self):
        print("hello,pytest")

    @pytest.mark.run(order=99)
    @pytest.mark.skipif(age>18,reason="年龄已经大于18，不用测啦")
    def testAge(self):
        print("myage")


    @pytest.mark.skip(reas="不想测试这个啦")
    def testAddr(self):
        print("myaddr")

  

'''  输出
testcase/test_01.py::TestName::testAge myage PASSED testcase/test_01.py::TestName::testName hello,pytest PASSED testcase/test_01.py::TestName::testAddr SKIPPED (不想测试这个啦)
'''
''' 当age=19时，输出
testcase/test_01.py::TestName::testAge SKIPPED (年龄已经大于18，不用测啦) testcase/test_01.py::TestName::testName hello,pytest PASSED testcase/test_01.py::TestName::testAddr SKIPPED (不想测试这个啦)
'''
```







