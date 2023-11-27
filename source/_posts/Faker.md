---
title: Faker生成测试伪数据
date: 2023-11-26 17:21:21
tags: [faker,'测试','测试数据']
---

### 测试数据

​		1）测试数据的来源

- 人工手动编写测试数据
- 在数据库中直接插入数据<!-- more -->
- csv文件
- 通过wbe ui反复点击，生成对应的数据
- 利用postman、jmeter
- 随机生成

​		2）问题

​		虽然有各种各样的方法能生成测试数据，但核心思想大都是人为设计测试数据的生成。当数据量大的时候，该工作量复杂且繁琐。且生成的数据可能失去了它对应的意义（如地址随机生成为了一串数字）。因此，faker库就出现在了人们面前（不是大魔王啊）

### Faker库

​		1）简介

​		Faker是Python的一个第三方库，用于生成虚拟数据。它支持全球各地的地名、职业、性别等数据生成。Faker库的核心功能是通过强大的生成算法，随机生成真实世界中的类似数据。Faker库广泛应用于数据测试、数据清洗和数据填充等领域。

​		2）安装

​		在python对应项目的虚拟环境中执行以下命令即可（python是真的方便）。

```python
pip install Faker #安装

faker --version   #检查
```

​		3）使用

```python
from faker import Faker

fk=Faker(locale="zh_Cn")#明人不说暗话，都是中国人
#使用多种语言 fk=Faker(locale=["zh_Cn","zh_TW"])

print(fk.name())  #生成名字，按找设置的地区检查
print(fk.name_female())  #生成女性名字
print(fk.name_male())   #生成男性名字   董冬梅  这也不太像啊，哈哈哈
print(fk.phone_number()) #生成手机号，会按照你设定的地区，自动检查格式
print(fk.phonenumber_prefix()) #随机生成一个手机号号段  前几位
print(fk.ssn())  #生成一个的身份ID
print(fk.date_of_birth())   #生成一个生日日期
print(fk.date_of_birth(minimum_age=18, maximum_age=20))  #生成一个生日日期(18~20岁之间)
print(fk.credit_card_number())  #生成一个信用卡号
print(fk.license_plate())  #生成一个车牌号
print(fk.bank())  #生成一个银行名称
print(fk.email())  #生成一个邮箱
print(fk.company())  #生成一个完整的公司名称
print(fk.company_suffix())   #生成一个公司名
print(fk.job())  #生成一个职位名称
```

​		4）内置的各种函数

```python
from faker import Faker
from collections import OrderedDict
# fake = Faker(["en_US", "zh_CN", "ja_JP"])
fake = Faker(["zh_CN"])
# print(fake.name())
# print(fake['en-US'].name())
# print(fake.company())
 
# 1、使用？#自定义规则，随机生成字符串
print(fake.bothify())  # 默认生成字符串格式： 05 RW
print(fake.bothify(text="666????####", letters='我们的家'))  # letters的字符串随机给text的？使用，##默认数字代替，格式如： 我我的我4777
 
# 2、使用^自定义规则，随机生成16进制字符串
print(fake.hexify(text='MAC Address: ^^:^^:^^:^^:^^:^^', upper=True))  # MAC Address: CD:18:FC:9F:B6:49
 
# 3、随机生成 i18n 语言的代码
print(fake.language_code())  # yo
 
# 4、使用？自定义规则，随机生成ASCII字符串
print(fake.lexify(text='Random Identifier: ??????????', letters='我ABCDE'))  # Random Identifier: CBC我DD我ABE
 
# 5、随机生成 i18n 区域设置
print(fake.locale())  # zh_CH
 
# 6、使用#！@%自定义规则，随机生成字符串
print(fake.numerify(text='#  @!! @ %'))  # 1  98  7  （#=[0,9] %=[1,9] !=随机数字或空字符 @=非0数字或空字符）
 
# 7、随机选择对象元素，并随机生成列表
print(fake.random_choices(elements=('a', 'b', 'c', 'd'), length=10))  # ['a', 'c', 'c', 'd', 'a', 'd', 'd', 'b', 'b', 'a']
print(fake.random_choices(elements=OrderedDict([("a", 0.45), ("b", 0.35), ("c", 0.15), ("d", 0.05), ])))  # ['b', 'c', 'a', 'a']
 
# 8、随机生成0-9整数
print(fake.random_digit())  # 0
 
# 9、随机生成1-9整数
print(fake.random_digit_not_null())  # 1
 
# 10、随机生成0-9整数或空值
print(fake.random_digit_or_empty())  # ""
 
# 11、随机选择元素，默认可重复、长度为1
print(fake.random_element(elements=('a', 'b', 'c', 'd')))  # a
print(fake.random_element(elements=OrderedDict([("a", 0.45), ("b", 0.35), ("c", 0.15), ("d", 0.05), ])))  # a
 
# 12、随机选择元素，默认可重复、长度不定
print(fake.random_elements(elements=('a', 'b', 'c', 'd'), unique=False))
print(fake.random_elements(elements=OrderedDict([("a", 0.45), ("b", 0.35), ("c", 0.15), ("d", 0.05), ]), length=20, unique=False))
 
# 13、在指定范围内，随机生成整数
print(fake.random_int(min=0, max=15, step=3))
 
# 14、随机生成ASCII字符串 [a-zA-Z]
print(fake.random_letter())  # 'y'
 
# 15、随机生成ASCII字符串列表 [a-zA-Z]
print(fake.random_letters(length=10))  # ['R', 'N', 'v', 'n', 'A', 'v', 'O', 'p', 'y', 'E']
 
# 16、随机生成ASCII小写字符串
print(fake.random_lowercase_letter())  # c
 
# 17、随机生成整数
'''
如果digits为None(默认值)，则取值范围为1 ~ 9之间的随机整数。
如果fix_len为False(默认值)，则可以生成所有不超过位数的整数。
如果fix_len为True，则只能生成具有精确位数的整数。
'''
print(fake.random_number(fix_len=True))  # 297371
print(fake.random_number(digits=3, fix_len=False))  # 577
 
# 18、随机生成元素不重复的不超出元素数量的列表
print(fake.random_sample(elements=('a', 'b', 'c', 'd', 'f', 'f'), length=6))  # 元素可相同，但length不能大于6
 
# 19、生成大写字母的ASCII字符串
print(fake.random_uppercase_letter())
 
# 20、随机生成接近某个数字的整数
'''
如果le为False(默认值)，则允许生成数量的140%。如果为True，则上限生成为100%。
如果ge为False(默认值)，则允许生成数量减少到60%。如果为True，下限生成上限为100%。
如果提供了最小值的数值，则生成的小于最小值的值将被固定在最小值。
如果为max提供了一个数值，则生成的大于max的值将被固定在max。
如果le和ge都为True，则number的值将自动返回，而不管提供的min和max的值是什么。
'''
print(fake.randomize_nb_elements(number=100))  # 83
print(fake.randomize_nb_elements(number=100, le=True, ge=True, min=80))  # 100
 
# 21、随机生成地址和邮政编号
print(fake.address())
 
# 22、随机生成门牌号
print(fake.building_number())
 
# 23、随机生成城市
print(fake.city())
 
# 24、随机生成特殊市
print(fake.city_suffix())
 
# 25、随机生成国家
print(fake.country())
 
# 26、随机生成国家编号
print(fake.country_code())
 
# 27、生成当前国家
print(fake.current_country())
 
# 28、生成当前国家编号
print(fake.current_country_code())
 
# 29、随机生成邮编
print(fake.postcode())
 
# 30、随机生成街道地址
print(fake.street_address())
 
# 31、随机生成街道名称
print(fake.street_name())
 
# 32、随机生成街道名称后缀
print(fake.street_suffix())
 
# 33、随机生成汽车供应商牌照
print(fake.license_plate())  # 974-XXRA
 
# 34、生成ABA的路由传输号
print(fake.aba())
 
# 35、生成银行提供商的ISO 3166-1 alpha-2国家代码
print(fake.bank_country())  # GB
 
# 36、生成基本银行帐号(BBAN)
print(fake.bban())  # MAAN00447407504564
 
# 37、生成国际银行账号(IBAN)
print(fake.iban())
 
# 38、生成SWIFT代码
print(fake.swift(length=11, primary=True, use_dataset=True))  # SVWBGBNKXXX
 
# 39、生成11位的SWIFT代码
print(fake.swift11(use_dataset=True))  # SVWBGBNKXXX
 
# 40、生成8位的SWIFT代码
print(fake.swift8(use_dataset=True))
 
# 41、生成EAN码
print(fake.ean(prefixes=('45', '49'), length=13))  # 4532804944052
 
# 42、生成EAN13码
print(fake.ean13(prefixes=('45', '49')))  # 4518561138095
 
# 43、生成EAN8码
print(fake.ean8(prefixes=('45', '49')))  # 45877841
 
# 44、生成指定长度的本地化EAN条码
print(fake.localized_ean(length=8))
 
# 45、生成指定长度的本地化EAN13条码
print(fake.localized_ean13())
 
# 46、生成指定长度的本地化EAN8条码
print(fake.localized_ean8())
 
# 47、生成随机颜色值
print(fake.color(hue='red'))
print(fake.color(luminosity='light'))
print(fake.color(hue=(100, 200), color_format='rgb'))
print(fake.color(hue='orange', luminosity='bright'))
print(fake.color(hue=135, luminosity='dark', color_format='hsv'))
print(fake.color(hue=(300, 20), luminosity='random', color_format='hsl'))
 
# 48、随机生成颜色名称
print(fake.color_name())
 
# 49、生成一个十六进制三元组格式的颜色
print(fake.hex_color())
 
# 50、生成一个以逗号分隔的RGB值格式的颜色
print(fake.rgb_color())
 
# 51、用CSS rgb()函数生成颜色格式
print(fake.rgb_css_color())
 
# 52、生成一个网络安全的颜色名称
print(fake.safe_color_name())
 
# 53、生成一个网络安全的颜色格式为十六进制三重
print(fake.safe_hex_color())
 
# 54、公司相关(技术\思想\名称...)
print(fake.bs())  # leverage plug-and-play networks
print(fake.catch_phrase())
print(fake.company())
print(fake.company_suffix())
 
# 55、信用卡相关
print(fake.credit_card_expire())  # 09/28
print(fake.credit_card_full())  # 'Discover\nKatherine Fisher\n6587647593824218 05/26\nCVC: 892\n'
print(fake.credit_card_number())  # 6504876475938248
print(fake.credit_card_provider())  # VISA 19 digit
print(fake.credit_card_security_code())  # 604
 
# 56、货币相关
print(fake.cryptocurrency())
print(fake.cryptocurrency_code())
print(fake.cryptocurrency_name())
print(fake.currency())
print(fake.currency_code())
print(fake.currency_name())
print(fake.currency_symbol())
print(fake.pricetag())
 
# 57、时间相关
print(fake.am_pm())
print(fake.century())
print(fake.date())
print(fake.date_between())
print(fake.date_between_dates())
print(fake.date_object())
print(fake.date_of_birth())
print(fake.date_this_century())
print(fake.date_this_decade())
print(fake.date_this_month())
print(fake.date_this_year())
print(fake.date_time())
print(fake.date_time_ad())
print(fake.date_time_between())
print(fake.date_time_between_dates())
print(fake.date_time_this_century())
print(fake.date_time_this_decade())
print(fake.date_time_this_month())
print(fake.date_time_this_year())
print(fake.day_of_month())
print(fake.day_of_week())
print(fake.future_date())
print(fake.future_datetime())
print(fake.iso8601())
print(fake.month())
print(fake.month_name())
print(fake.past_date())
print(fake.past_datetime())
print(fake.pytimezone())
print(fake.time())
print(fake.time_delta())
print(fake.time_object())
print(fake.time_series())
print(fake.timezone())
print(fake.unix_time())
print(fake.year())
 
# 58、文件相关
print(fake.file_extension())
print(fake.file_extension(category='image'))
print(fake.file_name(category='audio'))
print(fake.file_name(extension='abcdef'))
print(fake.file_name(category='audio', extension='abcdef'))
print(fake.file_path(depth=3))
print(fake.file_path(depth=5, category='video'))
print(fake.file_path(depth=5, category='video', extension='abcdef'))
print(fake.mime_type())
print(fake.mime_type(category='application'))
print(fake.unix_device())
print(fake.unix_device(prefix='mmcblk'))
print(fake.unix_partition())
print(fake.unix_partition(prefix='mmcblk'))
 
# 59、陆地坐标数据
print(fake.coordinate())
print(fake.latitude())
print(fake.latlng())
print(fake.local_latlng())
print(fake.location_on_land())
print(fake.longitude())
 
# 60、因特网相关
print(fake.ascii_company_email()) # 邮箱
print(fake.ascii_email())  # ascii邮箱
print(fake.ascii_free_email())
print(fake.ascii_safe_email())
print(fake.company_email())  # 公司邮箱
print(fake.dga())  # 网址
print(fake.domain_name())  # 网址
print(fake.domain_word())
print(fake.email())
print(fake.free_email())
print(fake.free_email_domain())
print(fake.hostname())
print(fake.http_method())  # http请求方法
print(fake.iana_id())  # IANA注册ID
print(fake.ipv4())  # 随机ip
print(fake.ipv4_network_class())  # 网络类别
print(fake.ipv4_private())
print(fake.ipv4_public())
print(fake.ipv6())
print(fake.mac_address())  # mac地址
print(fake.nic_handle())  # 网卡处理ID
print(fake.nic_handles())
print(fake.port_number())  # 端口号
print(fake.ripe_id())  # 组织ID
print(fake.safe_domain_name()) # 域名
print(fake.safe_email())  # 邮箱
print(fake.slug())  # Django算法
print(fake.tld())  # 域名后缀
print(fake.uri())  # http请求路径
print(fake.uri_extension())
print(fake.uri_page())  # 请求页面名
print(fake.uri_path())  # 资源路径
print(fake.url())  # url
print(fake.user_name())  # 用户名
 
# 61、isbn规则相关
print(fake.isbn10())
print(fake.isbn13())
 
# 62、工作的职位名称
print(fake.job())
 
# 63、文章相关
print(fake.paragraph(nb_sentences=5)) # 生成段落
print(fake.paragraph(nb_sentences=5, variable_nb_sentences=False))
print(fake.paragraph(nb_sentences=5, ext_word_list=['abc', 'def', 'ghi', 'jkl']))
print(fake.paragraph(nb_sentences=5, variable_nb_sentences=False, ext_word_list=['abc', 'def', 'ghi', 'jkl']))
print(fake.paragraphs(nb=5))  # 生成段落list
print(fake.sentence(nb_words=10))  # 生成一个句子
print(fake.sentence(nb_words=10, variable_nb_words=False))
print(fake.sentences())  # 生成句子list
print(fake.sentences(nb=5))
print(fake.text(max_nb_chars=20))  # 文本字符串
print(fake.text(max_nb_chars=80))
print(fake.text(max_nb_chars=160))
print(fake.text(ext_word_list=['abc', 'def', 'ghi', 'jkl']))
print(fake.texts(nb_texts=5))  # 文本字符串列表
print(fake.texts(nb_texts=5, max_nb_chars=50))
print(fake.texts(nb_texts=5, max_nb_chars=50, ext_word_list=['abc', 'def', 'ghi', 'jkl']))
print(fake.word())  # 词语字符串
print(fake.word(ext_word_list=['abc', 'def', 'ghi', 'jkl']))
print(fake.words())  # 词语列表
print(fake.words(nb=5, ext_word_list=['abc', 'def', 'ghi', 'jkl']))
print(fake.words(nb=4, ext_word_list=['abc', 'def', 'ghi', 'jkl'], unique=True))
 
# 数据类型相关
print(fake.binary(length=64))  # 创建字节
print(fake.boolean(chance_of_getting_true=75))  # 布尔类型
print(fake.csv(header=('Name', 'Address', 'Favorite Color'), data_columns=('{{name}}', '{{address}}', '{{safe_color_name}}'), num_rows=10, include_row_ids=True))  # 生成随机的逗号分隔值
print(fake.dsv(data_columns=('{{name}}', '{{address}}'), num_rows=5, delimiter='$'))  # 生成随机分隔符分隔值。
print(fake.fixed_width(data_columns=[(20, 'name'), (3, 'pyint', {'min_value':50, 'max_value':100})], align='right', num_rows=2))  # 生成随机固定宽度值
print(fake.image(size=(16, 16), hue=[90, 270], image_format='ico'))  # 使用Python图像库生成一张图片并在上面绘制一个随机的多边形。如果没有安装它，这个提供程序将无法运行。以给定格式返回表示图像的字节。
print(fake.json(data_columns=[('Name', 'name'), ('Points', 'pyint', {'min_value':50, 'max_value':100})], num_rows=1))  # 生成随机的JSON结构值
print(fake.md5(raw_output=False))  # 生成MD5数据
print(fake.null_boolean())  # 生成空值或布尔值
print(fake.password(length=12))  # 生成密码
print(fake.password(length=40, special_chars=False, upper_case=False))
print(fake.psv(header=('Name', 'Address', 'Favorite Color'), data_columns=('{{name}}', '{{address}}', '{{safe_color_name}}'), num_rows=10, include_row_ids=True))  # 生成随机的管道分隔值
print(fake.sha1(raw_output=False))  # 生成一个随机的SHA1哈希
print(fake.sha256(raw_output=False))  # 生成一个随机的SHA256哈希
print(fake.tar(uncompressed_size=256, num_files=32, min_file_size=4, compression='bz2'))  # 生成包含一个随机有效tar文件的字节对象。
print(fake.tsv(header=('Name', 'Address', 'Favorite Color'), data_columns=('{{name}}', '{{address}}', '{{safe_color_name}}'), num_rows=10, include_row_ids=True))  # 生成随机的制表符分隔值
print(fake.uuid4())  # 如果使用可调用对象指定，则生成一个随机UUID4对象并将其转换为另一种类型
print(fake.uuid4(cast_to=None))
print(fake.zip(uncompressed_size=256, num_files=32, min_file_size=4, compression='bz2'))  # 生成包含一个随机有效的zip归档文件的bytes对象。
 
# 人相关
print(fake.first_name())  # 人名
print(fake.first_name_female())  # 女名
print(fake.first_name_male())  # 男名
print(fake.first_name_nonbinary())
print(fake.language_name())
print(fake.last_name())
print(fake.last_name_female())
print(fake.last_name_male())
print(fake.last_name_nonbinary())
print(fake.name())
print(fake.name_female())
print(fake.name_male())
print(fake.name_nonbinary())
print(fake.prefix())
print(fake.prefix_female())
print(fake.prefix_male())
print(fake.prefix_nonbinary())
print(fake.suffix())
print(fake.suffix_female())
print(fake.suffix_male())
print(fake.suffix_nonbinary())
 
# 电话号码相关
print(fake.country_calling_code())  # 区号
print(fake.msisdn())
print(fake.phone_number())
 
# 个人信息相关
print(fake.profile())
print(fake.simple_profile())
 
# python相关（python数据类型）
print(fake.pybool())
print(fake.pydecimal())
print(fake.pydict())
print(fake.pyfloat())
print(fake.pyint())
print(fake.pyiterable())
print(fake.pylist())
print(fake.pyset())
print(fake.pystr())
print(fake.pystr_format())
print(fake.pystruct())
print(fake.pytuple())
 
# ssn
print(fake.ssn())  # 865-50-6891
 
# 默认用户代理相关、认证信息相关、通行证相关
print(fake.android_platform_token())
print(fake.chrome())
print(fake.firefox())
print(fake.internet_explorer())
print(fake.ios_platform_token())
print(fake.linux_platform_token())
print(fake.linux_processor())
print(fake.mac_platform_token())
print(fake.mac_processor())
print(fake.opera())
print(fake.safari())
print(fake.user_agent())
print(fake.windows_platform_token())
```

### Faker 测试

​		1）faker将数据导入数据库中

```python
import pymysql.cursors
#pip install pymysql
from faker import Faker

fk = Faker(locale="zh_CN")

class SQLFaker():
    def __init__(self):
        try://可能连接失败，放入try except中处理异常
            self.db = pymysql.connect(		# 连接数据库信息
                host="**.**.**.**",
                port=3306,
                user='root',
                password="password",
                database="test",
                charset="utf8mb4")  #在mysql中，utf8默认指的是utf8mb3，即使用1-3个字节表示一个字符；正常来说utf8也就是最大使用3个字节的utf8mb3已经够用了，但是为了存储4个字节场景下的字符如emoji表情，就需要用到4个字节编码的utf8，也就是utf8mb4。其中mb4的含义为：most bytes 4，也就是最大4字节。
        except pymysql.Error as e:
            print(f'连接数据库失败！{e}')

        self.cursor = self.db.cursor()  	# 使用cursor()方法创建一个游标对象，用于操作数据库

    def createDabate(self):
        try:
            sql = """CREATE TABLE `employees` (  
                    `userid` varchar(50) UNIQUE COMMENT '员工id',
                    `ename` VARCHAR (50) NOT NULL COMMENT '姓名',
                    `username` VARCHAR (20) NOT NULL COMMENT '用户名',
                    `password` VARCHAR (50) NOT NULL COMMENT '登录密码',
                    `gender` VARCHAR (50) DEFAULT '-1' COMMENT '1表示男，0表示女，-1表示未知',
                    `IDNum` VARCHAR (50) NOT NULL UNIQUE COMMENT '身份ID',
                    `phoneNum` VARCHAR (50) NOT NULL COMMENT '手机号码',
                    `email` VARCHAR (50) COMMENT '邮箱',
                    `birthday` DATE COMMENT '生日',
                    `createtime` DATETIME COMMENT '创建时间'
                    ) DEFAULT CHARSET = utf8mb4 COMMENT = '员工信息表'"""
            #多行字符串，常用于注释
            #写啥都行，不会报错
            self.cursor.execute(sql)  			 # 执行SQL语句，创建数据表
            print("employees表创建成功！")
        except pymysql.Error as e:
            if str(e).split(",")[0] == "(1050":  //提取错误码
            //可根据对应的报错信息自己自定义
                print("employees表已存在，将尝试直接插入数据！")
            else:
                print(f'创建数据表失败！{e}')  		# 否则抛出异常

    def insertFakedata(self, num):  //插入数据
        try:
            for i in range(num):
                sql = """INSERT INTO `employees` VALUES('%s','%s','%s','%s','%s','%s','%s','%s','%s','%s')""" % (  //字符串的拼接
                    fk.uuid4(),                     # 用户id
                    fk.name(),                      # 用户姓名
                    fk.user_name(),                 # 用户名
                    fk.password(),                  # 密码
                    fk.random_int(min=-1, max=1),   # 性别  //用对应的数字表示男女
                    fk.ssn(),                       # 身份ID
                    fk.phone_number(),              # 手机号码
                    fk.free_email(),                # 邮箱
                    fk.date_of_birth(minimum_age=20, maximum_age=30)    # 生日，20~30岁之间
                    fk.date_time_between_dates()	# 创建时间
                )
                self.cursor.execute(sql)	# 执行SQL语句，插入数据
                self.db.commit()  			# 提交插入的数据，若缺少即使执行成功，数据库也不会显示数据
            print(f"成功插入{num}条数据！")
        except pymysql.Error as e:
            print(f'插入数据失败！{e}')

        self.cursor.close()  				# 关闭游标对象
        self.db.close()  					# 关闭数据库

if __name__ == '__main__':
    SQLFaker().createDabate()  				# 执行创建数据库
    SQLFaker().insertFakedata(500)			# 执行插入500条数据

```

​		2）faker将数据导入excel中

```python
# 创建test4.py
from faker import Faker
from openpyxl import load_workbook
from openpyxl.styles import Font, PatternFill
from openpyxl.utils import get_column_letter
from openpyxl.styles.alignment import Alignment
from openpyxl.styles.borders import Border, Side

fk = Faker("zh_CN")

def save_to_excel():
    wb = load_workbook("E:/Fakerdata.xlsx")  	# 打开Excel文件
    sheet = wb[wb.sheetnames[0]]  				# 选择表1
    # 随机生成1000条数据，同时设置单元格样式
    for i in range(1000):
        for h in range(len(excel_header)):
            id = fk.random_number(digits=5)  # 随机生成5位数
            name = fk.name()  				 # 随机生成姓名
            ssn = fk.ssn()  				 # 随机生成身份ID
            phone = fk.phone_number()  		 # 随机生成手机号
            email = fk.email()  			 # 随机生成邮箱
            company = fk.company()  		 # 随机生成公司名称
            status = fk.words(ext_word_list=["在职", "已离职"], nb=1)[0]  # 在指定两个字中随机选择一个
            sheet.cell(row=i + 2, column=1, value=id)
            #第几行，第几列，对应的值
            sheet.cell(row=i + 2, column=2, value=name)
            sheet.cell(row=i + 2, column=3, value=ssn)
            sheet.cell(row=i + 2, column=4, value=phone)
            sheet.cell(row=i + 2, column=5, value=email)
            sheet.cell(row=i + 2, column=6, value=company)
            sheet.cell(row=i + 2, column=7, value=status)
    wb.save("E:/Fakerdata.xlsx")  # 保存Excel文件

if __name__ == '__main__':
    save_to_excel()
#可优先设置好格式信息，直接插入数据即可
```

​		3）faker作数据驱动

```python
#将数据先封装成对应的列表list，然后使用装饰器传入即可
#或者直接将对应的值改成faker生成的数据
from faker import Faker
import pytest
out=[]
faker=Faker(locale="zh_Cn")
for i in range(10):
    list=[]
    list.append(faker.name())
    list.append(faker.random_int(min=0,max=100))
    out.append(list)

class TestDDT:
    @pytest.mark.parametrize('name,age',out) //数据驱动
    def testFaker(self,name,age):
        print(name,age)

```
