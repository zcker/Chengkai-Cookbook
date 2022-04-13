---
cover: >-
  https://images.unsplash.com/photo-1489648022186-8f49310909a0?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwyfHxsaW5lfGVufDB8fHx8MTY0ODA0NDI5Ng&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 📈 折线图

### x轴为时间轴的折线图

对于x轴为时间，官方提供了例子做参考，如下：

{% embed url="https://echarts.apache.org/examples/zh/editor.html?c=area-time-axis" %}

{% embed url="https://codepen.io/sjcobb/pen/dyzWeKO" %}

这里注意的要点如下：

* 需要将type设置为time
* 设置解析时间的formatter，不然没法显示
* 传递数据参数的结构为二维数组

```javascript
xAxis: {
                type: 'time',    //important
                splitLine: {
                    show: false
                },
                axisLabel: { // 坐标轴标签样式设置
                    formatter: function (value, index) { // 坐标轴文字展示样式处理
                        // echarts自带的日期格式化方法
                        return echarts.format.formatTime('hh:mm:ss', value)
                    }
                },
            },
            
series的一个折线
                {
                    name: '往返时延',
                    type: 'line',
                    stack: 'Total',
                    data: rtt_dict    //类型是二维数组
                    //['time','data']
                },
```

#### **xAxis.axisLabel. formatter**

其中formatter详解如下

{% embed url="https://echarts.apache.org/zh/option.html#xAxis.axisLabel.formatter" %}

这里截取一部分关键点，可以粗略看一下：

![](<../../../.gitbook/assets/image (129).png>)
