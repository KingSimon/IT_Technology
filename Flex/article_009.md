# Parsley框架用AtionScript语言配置
在creationComplete事件中添加以下AS代码
```
FlexContextBuilder.build(ParsleyConfig, this);
```
代替MXML中
```
<parsley:ContextBuilder config="ParsleyConfig" />
```