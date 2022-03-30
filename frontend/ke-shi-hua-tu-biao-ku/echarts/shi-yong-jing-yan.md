# 🎥 使用经验

## toolbox

内置有[导出图片](https://echarts.apache.org/zh/option.html#toolbox.feature.saveAsImage)，[数据视图](https://echarts.apache.org/zh/option.html#toolbox.feature.dataView)，[动态类型切换](https://echarts.apache.org/zh/option.html#toolbox.feature.magicType)，[数据区域缩放](https://echarts.apache.org/zh/option.html#toolbox.feature.dataZoom)，[重置](https://echarts.apache.org/zh/option.html#toolbox.feature.reset)五个工具。

![配置效果](<../../../.gitbook/assets/image (16) (1) (1) (1).png>)

常用配置如下：

```javascript
toolbox: {
                show: true,
                feature: {
                    dataZoom: {
                        yAxisIndex: "none",
                    },
                    dataView: {readOnly: false},
                    restore: {},
                    saveAsImage: {},
                },
            },
```

官方文档

{% embed url="https://echarts.apache.org/zh/option.html#toolbox.id" %}

## 加载旋转图

在初始化dom后添加`chart.hideLoading();`在 `setOption`前写 `chart.hideLoading();`

## 参考资料

* 官方配置介绍

{% embed url="https://echarts.apache.org/zh/option.html#title" %}

*
