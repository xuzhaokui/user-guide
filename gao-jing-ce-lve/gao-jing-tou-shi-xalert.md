# 告警透视

OpsMind 提供实时的告警分析功能帮助运维人员快速判断当前的告警热点，以加快故障定位的速度。

进入方式：点击左侧菜单中的 **透视**，在展开的子菜单中选择 **告警透视** 功能即可。

## 告警维度分析

在告警透视页面上方，可以选择时间范围与是否包含已恢复告警的选项。下方会根据会根据选择的条件将期间的告警进行维度分析：

![](../.gitbook/assets/x-alert-1.png)

其中蓝色的横条分别代表左侧说明的维度，蓝条的长度代表每个维度值占的比例，例如在 **级别** 一栏中，warn 级别的告警比 critical 级别的告警略多一些（warn 蓝条长度略长）。

其他维度以此类推。这张维度分析图可以帮助您；

1. 在发生大规模告警时，能快速找到告警的共性维度（比如是否都是一个集群等）
2. 对历史发生告警进行分析复盘，有针对性地对系统进行运维升级

## 告警列表

在告警维度分析图下方，是告警列表的表格。告警列表目前可按照两种方式进行展示：

1. 直接逐行展示单条告警（即默认的无聚合状态）
2. 将告警归类到服务器的标签上

![](../.gitbook/assets/alert-table.png)

对于无聚合状态，每条告警都会在表格中列出来：

![](../.gitbook/assets/alert-table-non-aggr.png)

若选中了某个服务器标签的维度（下图以集群标签为例），所有的告警会按照标签进行归类：

![](../.gitbook/assets/alert-table-aggr-by-cluster.png)

## 查看告警详情

点击告警列表中的告警名称，可以进入告警的详情页面，在详情页面，您可以：

1. 查看告警的详细信息
2. 查看告警发生的时序图表
3. 对告警进行 ACK、静默、评论等操作

![](../.gitbook/assets/alert-insepct.png)
