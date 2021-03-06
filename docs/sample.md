实例：类似 WordPress 的博客网站
========

这里假设我们要建立一个类 WordPress 的博客网站，该怎么弄呢？

首先分析需求：

1. 博客网站自然需要发表和展示文章
2. 作者需要登录才能发表文章
3. 每篇文章都有不同的属性，比如：
    1. 标题
    2. 正文
    3. 作者
    4. 创建时间
    5. 标签

这里只为举例，所以不用列那么全，上面这些都很有代表性。第一项很明显不用管；第二项作为基础服务 Octopus 自带；我们只需要处理第三项。接下来，创建第一个表，`article`，结构如下：

| 字段 | 类型 | 说明 |
| ---- | ---- | ---- |
| id | int | 文章id |
| title | varchar(100) | 标题 |
| author | smallint | 作者id |
| create_time | datetime | 创建时间 |
| tags | tinyint | tag id |
| status | tinyint | 状态 |

表结构可以随时调整，所以也不必太去深究（正在运行的服务贸然调整数据库结构可能导致严重后果，此文章只为介绍框架，某些内容不能作为示范，相信大家可以自行分辨）。点击 `author` 字段后面的“关联”按钮，选中 `user` 表的 `id`，这样文章作者就和用户关联起来了。

接下来建立第二个表，`tag`，结构如下：

| 字段 | 类型 | 说明 |
| ---- | ---- | ---- |
| id | int | 标签id |
| tag | varchar(100) | 标签 |
| alias | varchar(100) | 标签别名，用来作为英文目录 |
| status | tinyint| 状态 |

回到刚才的 `article` 表，点击 `tags` 字段后面的“关联”按钮，选中 `tag` 表中的 `id`，注意，这次需要勾选“多对多”，因为一篇文章可以有多个标签。

以上建表的过程可以使用任何熟悉的工具完成，只要使用 Octopus 建立关联即可。

结构可以随时调整，所以也不必太去深究（正在运行的服务贸然调整数据库结构可能导致严重后果，此文章只为介绍框架，某些部分不作示范，相信大家可以自行分辨）。

## 配置API

现在盘点一下我们需要的API：

1. 登录/注册（已实现）
2. 创建文章
3. 读取文章，包括按照各种方式筛选排序

熟悉 RESTful 的人到这里应该已经明白了。接下来我们开始实操。

创建接口 `/artcile`，方法选择 `post`，操作选择“创建”，然后选择 `article` 表。对应的字段自然会到对应的字段去，这点不用担心。

再次创建接口 `/article`，方法选择 `get`，操作选择“读取”。选择 `article` 表，勾选您希望输出的字段。然后您可以继续定义筛选的字段和排序的字段，并且设置每页的数量，这些在 UI 上展现都很明确，就不一一说明了。
