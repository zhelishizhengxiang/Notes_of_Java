
# 学习和笔记相关

笔者学习Elasticsearch参考的视频教程为黑马程序员ES[视频链接](https://www.bilibili.com/video/BV1b8411Z7w5/?spm_id_from=333.337.search-card.all.click)，之后进一步观看了黑马程序员微服务开发中的ES[视频链接](https://www.bilibili.com/video/BV1S142197x7/?spm_id_from=333.337.search-card.all.click)的内容补充了部分笔记。笔记为公众号获取资料中的讲义为基础，添加了重点内容的细化和一些本人的理解。

如果时间比较紧只需要看黑马程序员ES[视频链接](https://www.bilibili.com/video/BV1b8411Z7w5/?spm_id_from=333.337.search-card.all.click)，微服务开发中的ES章节内容[视频链接](https://www.bilibili.com/video/BV1S142197x7/?spm_id_from=333.337.search-card.all.click)与其高度重合，不一样的地方看笔记即可全部明白。

由于百度网盘下载缓慢，此处提供学习所必须的相关资料，具体见[附件相关](#附件相关)
# 前置知识相关

1. 由于该教程的es安装和配置都是使用docker来做，所以需要学习docker。需要快速上手可见参考[02Docker](../Spring%20Cloud/黑马微服务/02Docker.md)。
2. 教程使用的demo项目是spring项目，需要学习可以参考`SpringBoot/`下的笔记。
3. 教程后期会涉及es与mysql的数据同步问题，所以需要学习消息队列的基础知识。本教程选用的消息队列为RabbitMQ，快速上手可参考[08消息队列——RabbitMQ初级](../Spring%20Cloud/黑马微服务/08消息队列——RabbitMQ初级.md)


# 附件相关

本教程所有需要的所有资料都在`ElasticSearch/archive/`下，接下来对该目录下文件做一定说明。

1. `es.tar,kibana.tar`：Elasticsearch和kibana的docker镜像压缩包。kibana为es的可视化客户端，书写DSL有对应提示，比PostMan更高效。
2. `es/`：ik分词器插件
3. `hotel-demo/`：学习所需要的demo工程
4. `tb_hotel.sql`：demo工程所需要的数据库脚本
5. `py/`：拼音分词器插件
6. `hotel-admin/`：实现数据同步的另一个生产者工程
7. `docker-compose.yml`：用于构建ES集群的docker-compose文件
8. `cerebro-0.9.4.zip`：用于监控ES集群状态的应用压缩包
9. `dsl.json` ：包含教程用到的所有DSL语句

> 注：由于`es.tar,kibana.tar`文件过大，所以此处使用Git LFS进行存储