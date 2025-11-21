# 关于怎么prepare

建议prepare得花4个月的时间

有事没事都多去看看人家大佬们是怎么解决问题的，然后思考一下人家公司遇到什么问题

[Engineering at Meta](https://engineering.fb.com/), [Meta Research](https://research.fb.com/), [AWS Architecture Blog](https://aws.amazon.com/blogs/architecture/), [Amazon Science Blog](https://www.amazon.science/blog), [Netflix TechBlog](https://netflixtechblog.com/), [Google Research](https://research.google/), [Engineering at Quora](https://quoraengineering.quora.com/), [Uber Engineering Blog](https://eng.uber.com/), [Databricks Blog](https://databricks.com/blog/category/engineering), [Pinterest Engineering](https://medium.com/@Pinterest_Engineering), [BlackRock Engineering](https://medium.com/blackrock-engineering), [Lyft Engineering](https://eng.lyft.com/), [Salesforce Engineering](https://engineering.salesforce.com/).

有事没事也要思考一下身边常见的应用是怎么工作的。

有时间还得思考一下不同的component用上去会有什么效果，譬如firebase vs sql

再有时间试试build个side project，重现一下人家的系统，看你会遇到什么问题

requirement有两种，functional和non-functional

注意思考一下问题：

1. data size，growth，keep多久，regulatory compliance
2. data怎么被consume的，by user or downstream system
3. data consistency，eventually consistent ok or not

