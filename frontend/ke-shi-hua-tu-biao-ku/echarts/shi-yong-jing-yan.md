# ð¥ ä½¿ç¨ç»éª

## toolbox

åç½®æ[å¯¼åºå¾ç](https://echarts.apache.org/zh/option.html#toolbox.feature.saveAsImage)ï¼[æ°æ®è§å¾](https://echarts.apache.org/zh/option.html#toolbox.feature.dataView)ï¼[å¨æç±»ååæ¢](https://echarts.apache.org/zh/option.html#toolbox.feature.magicType)ï¼[æ°æ®åºåç¼©æ¾](https://echarts.apache.org/zh/option.html#toolbox.feature.dataZoom)ï¼[éç½®](https://echarts.apache.org/zh/option.html#toolbox.feature.reset)äºä¸ªå·¥å·ã

![éç½®ææ](<../../../.gitbook/assets/image (83).png>)

å¸¸ç¨éç½®å¦ä¸ï¼

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

å®æ¹ææ¡£

{% embed url="https://echarts.apache.org/zh/option.html#toolbox.id" %}

## å è½½æè½¬å¾

å¨åå§ådomåæ·»å `chart.hideLoading();`å¨ `setOption`åå `chart.hideLoading();`

## åèèµæ

* å®æ¹éç½®ä»ç»

{% embed url="https://echarts.apache.org/zh/option.html#title" %}

*
