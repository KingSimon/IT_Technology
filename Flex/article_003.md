# 国际化处理
编译命令

-locale en_US zh_CN -source-path=locale/{locale} -keep-all-type-selectors=true

message.properties 文档格式写法

enter_key=showmessage

调用方式

mxml格式调用：`@Resource(key='enter_key',bundle='message')`

as格式调用：`ResourceManager.getInstance().getString("message", "enter_key");`