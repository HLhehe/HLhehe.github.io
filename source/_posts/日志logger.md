---
title: 日志logger
date: 2023-11-25 17:21:21
tags: ['日志',logger]
---

### 日志

​		1）什么是日志

​		日志（Log）是指记录程序运行状态的信息。在程序运行过程中，它可以记录代码中变量的变化情况，跟踪代码运行时的轨迹，向文件或控制台输出代码的调试信息等。

<!-- more -->

​		2）特点

1. 调试程序
2. 定位跟踪bug
3. 根据日志，查看系统运行是否出错
4. 分析用户行为，与数据统计

​		3）日志级别	

- DEBUG 调试级别：打印非常详细的日志信息，通常用于对代码的调试
- INFO 信息级别：打印一般的日志信息，突出强调程序的运行过程
- WARNING 警告级别：打印警告日志信息，表明会出现潜在错误的情形，一般不影响软件的正常使用
- ERROR 错误级别：打印错误异常信息，该级别的错误可能会导致系统的一些功能无法正常使用
- CRITICAL 严重错误级别：一个严重的错误，这表明系统可能无法继续运

​		4）说明

- 上面的日志级别是从上到下依次升高的，即：DEBUG < INFO < WARNING < ERROR < CRITICAL

- 当为程序指定一个日志级别后，程序会记录所有日志级别大于或等于指定日志级别的日志信息，而不是仅仅记录指定级别的日志信息

- 开发常用级别DEBUG、INFO、WARNING、ERROR ；测试常用级别：INFO、ERROR

### logging

​		Python中有一个标准库模块logging可以直接记录日志

​		1）基本用法

```python
import logging

logging.debug("这是一条调试信息")
logging.info("这是一条普通信息")
logging.warning("这是一条警告信息")
logging.error("这是一条错误信息")
logging.critical("这是一条严重错误信息")

'''输出
WARNING:root:这是一条警告信息
ERROR:root:这是一条错误信息
CRITICAL:root:这是一条严重错误信息
'''
'''why
默认的日志级别为warning，故只输出warning和高级别日志
'''
```

​		2）更改日志级别

```python
logging.basicConfig(level=logging.DEBUG)
#这样就会输出debug以上的日志信息，即所有日志信息
#注意要再使用日志之前就更改日志级别，否则还是默认的warning级别
```

​		注意：

- 在开发环境和测试环境中，为了尽可能详细的查看程序的运行状态来保证上线后的稳定性，可以使用DEBUG或INFO级别的日志获取

  详细的日志信息，这是非常耗费机器性能的。

- 在生产环境中，通常只记录程序的异常信息、错误信息等（设置成WARNING或ERROR级别），这样既可以减小服务器的I/O压力，也

  可以提高获取错误日志信息的效率和方便问题的排查。

​		3）日志格式

​		默认的日志的格式为：日志级别:Logger名称:日志内容

```python
#自定义日志格式
logging.basicConfig(format="%(levelname)s:%(name)s:%(message)s")
```

​		format中用到的占位符字符串格式

|       占位符        |                            描述                            |
| :-----------------: | :--------------------------------------------------------: |
|      %(name)s       |                        Logger的名字                        |
|     %(levelno)s     |                     数字形式的日志级别                     |
|  **%(levelname)s**  |                     文本形式的日志级别                     |
|    %(pathname)s     |        调用日志输出函数的模块的完整路径名，可能没有        |
|  **%(filename)s**   |               调用日志输出函数的模块的文件名               |
|     %(module)s      |                  调用日志输出函数的模块名                  |
|    %(funcName)s     |                  调用日志输出函数的函数名                  |
|   **%(lineno)d**    |             调用日志输出函数的语句所在的代码行             |
|     %(created)f     |        当前时间，用UNIX标准的表示时间的浮 点数表示         |
| %(relativeCreated)d |         输出日志信息时的，自Logger创建以来的毫秒数         |
|   **%(asctime)s**   | 字符串形式的当前时间。默认格式是 “2003-07-08 16:49:45,896” |
|     %(thread)d      |                      线程ID。可能没有                      |
|   %(threadName)s    |                      线程名。可能没有                      |
|     %(process)d     |                      进程ID。可能没有                      |
|   **%(message)s**   |                       用户输出的消息                       |

```python
import logging
#常用格式
fmt = '%(asctime)s %(levelname)s [%(name)s] [%(filename)s(%(funcName)s:%(lineno)d)] - %(message)s'

logging.basicConfig(level=logging.INFO, format=fmt)
#注意，若写多条basicConfig只生效一条
#故将日志级别，格式，输出写在一条语句中

logging.debug("调试")
logging.info("信息")
logging.warning("警告")
logging.error("错误")
```

​		4）日志输出

​		默认情况下Python的logging模块将日志打印到了标准输出中（控制台）

```python
#将日志输出到文件中
import logging

fmt = '%(asctime)s %(levelname)s [%(name)s] [%(filename)s(%(funcName)s:%(lineno)d)] - %(message)s'
logging.basicConfig(filename="a.log", level=logging.INFO, format=fmt)
#同时指定日志级别，日志格式，日志输出
#这种方式写入文件，中文乱码

logging.debug("调试")
logging.info("信息")
logging.warning("警告")
logging.error("错误")
```

​		注意：通过以上操作，可以进行基本的日志操作。但是如何将日志文件同时输出到文件和控制台呢？将警告日志输出到控制台，错误日志输出到文件中该如何操作呢？日志文件过大如何处理呢？

### logger 高阶

​		1）logger 4大模块

- 日志器———Logger———使用日志的入口
- 处理器———Handler———日志的输出，即输出到控制台还是文件中
- 格式器 ———Formatter ———决定日志记录的最终输出格式
- 过滤器 ———Filter——— 提供了更细粒度的控制工具来决定输出哪条日志记录，丢弃哪条日志记录

​		2）模块间的关系

- 日志器（logger）需要通过处理器（handler）将日志信息输出到目标位置，如：文件、sys.stdout、网络等
- 不同的处理器（handler）可以将日志输出到不同的位置
- 日志器（logger）可以设置多个处理器（handler）将同一条日志记录输出到不同的位置
- 每个处理器（handler）都可以设置自己的格式器（formatter）实现同一条日志以不同的格式输出到不同的地方。
- 每个处理器（handler）都可以设置自己的过滤器（filter）实现日志过滤，从而只保留感兴趣的日志

​		简单点说就是：日志器（logger）是入口，真正干活儿的是处理器（handler），处理器（handler）还可以通过过滤器（filter）和格式器（formatter）对要输出的日志内容做过滤和格式化等处理操作。

#### Logger 类

​		1）使用

```python
logger = logging.getLogger()
logger = logging.getLogger("myLogger")
```

​		logging.getLogger()方法有一个可选参数name，该参数表示将要返回的日志器的名称标识，如果不提供该参数，则返回root日志器

对象。 若以相同的name参数值多次调用getLogger()方法，将会返回指向同一个logger对象的引用（类似单例的感觉，**就是采用相同的名字多次调用返回的是同一个对象**）。

​		2）方法

```python
logger.debug()
logger.info()
logger.warning()    #输出日志
logger.error()
logger.critical()

logger.setLevel()   #设置日志器将会处理的日志消息的最低级别
logger.addHandler() #为该logger对象添加一个handler对象
logger.addFilter()  #为该logger对象添加一个filter对象
```

#### Handler 类

​	Handler对象的作用是将消息分发到handler指定的位置，比如：**控制台、文件、网络、邮件**等。 Logger对象可以通过addHandler()方

法为自己添加**多个**handler对象。

​		1）使用

​	在程序中不应该直接实例化和使用Handler实例，因为Handler是一个基类，它只定义了Handler应该有的接口。 应该使用Handler实

现类来创建对象，logging中内置的常用的Handler包括：

- **logging.StreamHandler**       		  将日志消息发送到输出到Stream，如std.out, std.err或任何file-like对象,控制台输出。
- logging.FileHandler              		    将日志消息发送到磁盘文件，默认情况下文件大小会无限增长
- **logging.handlers.RotatingFileHandler** 			  将日志消息发送到磁盘文件，并支持日志文件按大小切割
- **logging.hanlders.TimedRotatingFileHandler**   将日志消息发送到磁盘文件，并支持日志文件按时间切割
- logging.handlers.HTTPHandler 							   将日志消息以GET或POST的方式发送给一个HTTP服务器
- logging.handlers.SMTPHandler						 	   将日志消息发送给一个指定的email地址

​		2）方法

```python
handler.setLevel() #设置handler将会处理的日志消息的最低严重级别
handler.setFormatter() #为handler设置一个格式器对象
handler.addFilter() #为handler添加一个过滤器对象
```

#### Formatter 类

```python
#使用
formatter = logging.Formatter(fmt=None, datefmt=None, style='%')
#fmt：指定消息格式化字符串，如果不指定该参数则默认使用message的原始值
#datefmt：指定日期格式字符串，如果不指定该参数则默认使用"%Y-%m-%d %H:%M:%S"
#style：Python 3.2新增的参数，可取值为 '%', '{'和 '$'，如果不指定该参数则默认使用'%'
```

​		Filter 类使用较少，可自行查阅

### logger 应用

​		1）将日志同时输出到控制台和文件中。

```python
import logging

logger = logging.getLogger()  #初始化日志文件
#自定义格式
fmt = '%(asctime)s %(levelname)s [%(name)s] [%(filename)s(%(funcName)s:%(lineno)d)] - %(message)s'
formatter = logging.Formatter(fmt) #定义格式器

sh = logging.StreamHandler() #指定输出的handler，输出到控制台
sh.setFormatter(formatter) #将格式器加入到控制器中
logger.addHandler(sh)  #添加控制器

fh = logging.FileHandler("./b.log",encoding="utf-8")  #输出到文件的控制器,设置utf-8格式，中文不乱码
fh.setFormatter(formatter)

logger.addHandler(fh)
```

​		2）每日生成一个文件

```python
fh = logging.handlers.TimedRotatingFileHandler(filename, when='h', interval=1, backupCount=0)
#使用按找时间片分隔文件的控制器实现类
"""
将日志信息记录到文件中，以特定的时间间隔切换日志文件。
filename: 日志文件名
when: 时间单位，可选参数
	S - Seconds
	M - Minutes
	H - Hours
	D - Days，按照日期
	midnight - 每晚12点，天
	W{0-6} -  0 - Monday，周几
interval: 时间间隔，就代表几个when
backupCount: 日志文件备份数量。如果backupCount大于0，那么当生成新的日志文件时，将只保留backupCount个文件，删除最老的文件。
"""
```

```python
import logging.handlers #使用高级的处理器必须导这个包，不然报错

logger = logging.getLogger()
logger.setLevel(logging.DEBUG)#总的日志级别
# 日志格式
fmt = "%(asctime)s %(levelname)s [%(filename)s(%(funcName)s:%(lineno)d)] - %(message)s"
formatter = logging.Formatter(fmt)
# 输出到文件，每日一个文件
fh = logging.handlers.TimedRotatingFileHandler("./a.log", when='MIDNIGHT', interval=1, backupCount=31,encoding="utf-8")#注意都设置utf-8格式
#每天分隔一个文件，保留31个
fh.setFormatter(formatter)
fh.setLevel(logging.INFO)#可以文件中记录的日志级别
#控制器的级别应该大于等于日志器的级别，否则为日志器的级别
#日志器默认为warning
logger.addHandler(fh)

logger.debug("我真帅")
logger.info("我真棒")
#文件中只会存储我真棒
```

​		3）单例模式

```python
import logging.handlers #使用高级的处理器必须导这个包，不然报错

class GetLogger:
    logger=None;

    @classmethod
    def getLogger(cls):
        if cls.logger is None:
            cls.logger = logging.getLogger()
            cls.logger.setLevel(logging.DEBUG)
			# 日志格式
            fmt = "%(asctime)s %(levelname)s [%(filename)s(%(funcName)s:%(lineno)d)] - %(message)s"
            formatter = logging.Formatter(fmt)
   			 # 输出到文件，每日一个文件
            fh = logging.handlers.TimedRotatingFileHandler("./a.log", when='MIDNIGHT', interval=1, backupCount=31)   #每天分隔一个文件，保留31个
            fh.setFormatter(formatter)
            fh.setLevel(logging.INFO)#可以设置记录的日志级别
            cls.logger.addHandler(fh)
            sh = logging.StreamHandler() #指定输出的handler，输出到控制台
			sh.setFormatter(formatter) #将格式器加入到控制器中
			cls.logger.addHandler(sh)  #添加控制器

        return cls.logger
    
#单例很重要，奥里给
```

​		4）yml文件

```yaml
version: 1
disable_existing_loggers: false
formatters:
  simpleFormatter:
    format: '%(asctime)s %(levelname)s [%(filename)s(%(funcName)s:%(lineno)d)] - %(message)s'
handlers:
  consoleHandler:
    class: logging.StreamHandler
    level: DEBUG
    formatter: simpleFormatter
    stream: ext://sys.stdout
  fileHandler:
    class: logging.handlers.RotatingFileHandler
    level: DEBUG
    formatter: simpleFormatter
    filename: sample.log
    maxBytes: 80
    backupCount: 3
loggers:
  sampleLogger:
    level: DEBUG
    handlers: [fileHandler, consoleHandler]
    propagate: no
root:
  level: INFO
  handlers: [consoleHandler]

```

```python
import logging.config
import yaml

with open('./logconf/log.yaml', 'r') as f:
    config = yaml.safe_load(f.read())
    logging.config.dictConfig(config)

logger_r = logging.getLogger()  #使用root日志
logger_s = logging.getLogger('sampleLogger') #使用指定的日志，sampleLogger
```

