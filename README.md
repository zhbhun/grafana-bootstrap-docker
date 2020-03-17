
# Default sitespeed.io dashboards for Graphite

Run this container to get a default Graphite datasource linked with the default sitespeed.io dashboards.

The following environment variables can be set:

* *GF_USER* - Grafana user that has privileges to setup dashboards & datasources (default: sitespeedio)
* *GF_PASSWORD* - password for the Grafana user (default: hdeAga76VG6ga7plZ1)
* *GF_API*  - path to the Grafana API (default: ["http://grafana:3000/api"](http://grafana:3000/api)).
* *BACKEND* - Choose between graphite or influxdb (default: graphite) 

Example: `docker run -it --link graphite_grafana_1:grafana --network graphite_default sitespeedio_dashboards`

# Add a new dashboard

1. Export JSON from Grafana (see [Grafana Doc](http://docs.grafana.org/reference/export_import/)) and put it in the /dashboards folder
1. Rebuild the container `docker build -t sitespeedio_dashboards .`
1. Run the container `docker run -it --link <name of your grafana container>:grafana --network <name of your grafana network> sitespeedio_dashboards`

---

在源码基础上做了以下几点调整

1. 去掉了 webpagetest 的配置数据。
2. Page Summary 增加了 "Performance Grade" 指标。

---

## 配置说明

- id：唯一标识
- type：类型

    - graph

- title：标题
- description：描述
- transparent：是否透明

### graph - 图表

数据

- targets：{}[]

    - refId：A、B、C。。。
    - target：

        - alias

            - [alias](https://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.alias)：Takes one metric or a wildcard seriesList and a string in quotes. Prints the string instead of the metric name in the legend.
            - [aliasByMetric](https://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.aliasByMetric)：Takes a seriesList and applies an alias derived from the base metric name.
            - [aliasByNode](https://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.aliasByNode)：Takes a seriesList and applies an alias derived from one or more “node” portion/s of the target name or tags. Node indices are 0 indexed.
            - [aliasSub](https://graphite.readthedocs.io/en/latest/functions.html#graphite.render.functions.aliasSub)：Runs series names through a regex search/replace.

        - calculate
        - combine
        - ...

位置

- gridPos

    - h：高度
    - w：宽，24等分
    - x：x 轴位置
    - y：y 轴位置

图表

- bars：true|false

    是否显示柱状图

- lines：true|false

    是否显示折线图

- lineWidth：number

    折线图线条粗细

- Staircase：true|false

    折线图按阶梯式渲染

- stack：true|false

    堆叠，按顺序累加各项数据

- percentage：true|false

    stack 为 true 时，表示各个累加部分的占用百分比

- nullPointMode：connected|null|null as zero

    表示没有数据的部分是否要连接，connected 的时候才能生成折线图

- seriesOverrides：{}[]

    - alias：别名
    - color：颜色
    - dashes：是否显示成虚线
    - dashLength：虚线长度
    - spaceLength：续线间隔
    - fillBelowTo：指标名，表示两个指标之间渲染一个带颜色的区域

坐标

- xaxis：X 轴

    - show：true|false，是否显示
    - mode：模式，"time" | "seris" | "histogram"
    - name：名称
    - buckets：？
    - values：[]，？

- yaxes：{}[]，Y 轴

    - show：true|false，是否显示
    - format：单位，可以是时间、长度等
    - label：标签
    - logBase：？
    - min：最小值
    - max：最大值
    - decimal：number，精度

- yaxis：Y 轴

    - align：true|false
    - alignLevel：number

说明

- legend

    - show：true|false，是否显示
    - alignAsTable：true|false，是否以表格显示
    - rightSide：true|false，右侧显示
    - width：number，右侧显示时的宽度
    - min：true|false，
    - max：true|false，
    - avg：true|false，
    - current：true|false，
    - total：true|false，
    - decimals：numner，精度
    - hideEmpty：true|false，是否隐藏空值
    - hideZero：是否隐藏非 0

其他

- options：可选配置

    - dataLinks：{}[]，数据连接

        - url：地址
        - title：标题
        - targetBlank：true|false
