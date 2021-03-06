---
cover: >-
  https://images.unsplash.com/photo-1646613571756-6ab59aaa655f?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE2NDc1ODMxMDI&ixlib=rb-1.2.1&q=85
coverY: 0
---

# ðª with openæ¹æ³

## ç®ä»

Pythonåç½®äºè¯»åæä»¶çå½æ°ï¼ç¨æ³åCæ¯å¼å®¹çã

ç±äºæä»¶è¯»åæ¶é½æå¯è½äº§ç`IOError`ï¼ä¸æ¦åºéï¼åé¢ç`f.close()`å°±ä¸ä¼è°ç¨ãæä»¥ï¼ä¸ºäºä¿è¯æ è®ºæ¯å¦åºéé½è½æ­£ç¡®å°å³é­æä»¶ï¼æä»¬å¯ä»¥ä½¿ç¨`try ... finally`æ¥å®ç°ï¼

```python
try:
    f = open('/path/to/file', 'r')
    print(f.read())
finally:
    if f:
        f.close()
```

ä½æ¯æ¯æ¬¡é½è¿ä¹åå®å¨å¤ªç¹çï¼æä»¥ï¼Pythonå¼å¥äº`with`è¯­å¥æ¥èªå¨å¸®æä»¬è°ç¨`close()`æ¹æ³ã

è¿ååé¢ç`try ... finally`æ¯ä¸æ ·çï¼ä½æ¯ä»£ç æ´ä½³ç®æ´ï¼å¹¶ä¸ä¸å¿è°ç¨`f.close()`æ¹æ³ã

## åºç¨

```python
with open(r'filename.txt') as f:
   data_user=pd.read_csv(f)  #æä»¶çè¯»æä½

with open('data.txt', 'w') as f:
   f.write('hello world')  #æä»¶çåæä½
```

![](../../../.gitbook/assets/å¾ç.png)

{% hint style="info" %}
```
r:	ä»¥åªè¯»æ¹å¼æå¼æä»¶ãæä»¶çæéå°ä¼æ¾å¨æä»¶çå¼å¤´ãè¿æ¯**é»è®¤æ¨¡å¼**ã
rb: ä»¥äºè¿å¶æ ¼å¼æå¼ä¸ä¸ªæä»¶ç¨äºåªè¯»ãæä»¶æéå°ä¼æ¾å¨æä»¶çå¼å¤´ãè¿æ¯é»è®¤æ¨¡å¼ã
r+: æå¼ä¸ä¸ªæä»¶ç¨äºè¯»åãæä»¶æéå°ä¼æ¾å¨æä»¶çå¼å¤´ã
rb+:ä»¥äºè¿å¶æ ¼å¼æå¼ä¸ä¸ªæä»¶ç¨äºè¯»åãæä»¶æéå°ä¼æ¾å¨æä»¶çå¼å¤´ã
w:	æå¼ä¸ä¸ªæä»¶åªç¨äºåå¥ãå¦æè¯¥æä»¶å·²å­å¨åå°å¶è¦çãå¦æè¯¥æä»¶ä¸å­å¨ï¼åå»ºæ°æä»¶ã
wb:	ä»¥äºè¿å¶æ ¼å¼æå¼ä¸ä¸ªæä»¶åªç¨äºåå¥ãå¦æè¯¥æä»¶å·²å­å¨åå°å¶è¦çãå¦æè¯¥æä»¶ä¸å­å¨ï¼åå»ºæ°æä»¶ã
w+:	æå¼ä¸ä¸ªæä»¶ç¨äºè¯»åãå¦æè¯¥æä»¶å·²å­å¨åå°å¶è¦çãå¦æè¯¥æä»¶ä¸å­å¨ï¼åå»ºæ°æä»¶ã
wb+:ä»¥äºè¿å¶æ ¼å¼æå¼ä¸ä¸ªæä»¶ç¨äºè¯»åãå¦æè¯¥æä»¶å·²å­å¨åå°å¶è¦çãå¦æè¯¥æä»¶ä¸å­å¨ï¼åå»ºæ°æä»¶ã
a:	æå¼ä¸ä¸ªæä»¶ç¨äºè¿½å ãå¦æè¯¥æä»¶å·²å­å¨ï¼æä»¶æéå°ä¼æ¾å¨æä»¶çç»å°¾ãä¹å°±æ¯è¯´ï¼æ°çåå®¹å°ä¼è¢«åå¥å°å·²æåå®¹ä¹åãå¦æè¯¥æä»¶ä¸å­å¨ï¼åå»ºæ°æä»¶è¿è¡åå¥ã
ab:	ä»¥äºè¿å¶æ ¼å¼æå¼ä¸ä¸ªæä»¶ç¨äºè¿½å ãå¦æè¯¥æä»¶å·²å­å¨ï¼æä»¶æéå°ä¼æ¾å¨æä»¶çç»å°¾ãä¹å°±æ¯è¯´ï¼æ°çåå®¹å°ä¼è¢«åå¥å°å·²æåå®¹ä¹åãå¦æè¯¥æä»¶ä¸å­å¨ï¼åå»ºæ°æä»¶è¿è¡åå¥ã
a+:	æå¼ä¸ä¸ªæä»¶ç¨äºè¯»åãå¦æè¯¥æä»¶å·²å­å¨ï¼æä»¶æéå°ä¼æ¾å¨æä»¶çç»å°¾ãæä»¶æå¼æ¶ä¼æ¯è¿½å æ¨¡å¼ãå¦æè¯¥æä»¶ä¸å­å¨ï¼åå»ºæ°æä»¶ç¨äºè¯»åã
ab+:ä»¥äºè¿å¶æ ¼å¼æå¼ä¸ä¸ªæä»¶ç¨äºè¿½å ãå¦æè¯¥æä»¶å·²å­å¨ï¼æä»¶æéå°ä¼æ¾å¨æä»¶çç»å°¾ãå¦æè¯¥æä»¶ä¸å­å¨ï¼åå»ºæ°æä»¶ç¨äºè¯»åã
```
{% endhint %}

{% hint style="info" %}
file.read(\[size]) å°æä»¶æ°æ®ä½ä¸ºå­ç¬¦ä¸²è¿åï¼å¯éåæ°sizeæ§å¶è¯»åçå­èæ° file.readlines(\[size]) è¿åæä»¶ä¸­è¡åå®¹çåè¡¨ï¼sizeåæ°å¯é file.write(str) å°å­ç¬¦ä¸²åå¥æä»¶ file.writelines(strings) å°å­ç¬¦ä¸²åºååå¥æä»¶ file.close() å³é­æä»¶ file.closed è¡¨ç¤ºæä»¶å·²ç»è¢«å³é­ï¼å¦åä¸ºFalse

file.mode Accessæä»¶æå¼æ¶ä½¿ç¨çè®¿é®æ¨¡å¼ file.encoding æä»¶æä½¿ç¨çç¼ç  file.name æä»¶å file.newlines æªè¯»åå°è¡åéç¬¦æ¶ä¸ºNoneï¼åªæä¸ç§è¡åéç¬¦æ¶ä¸ºä¸ä¸ªå­ç¬¦ä¸²ï¼å½æä»¶æå¤ç§ç±»åçè¡ç»æç¬¦æ¶ï¼åä¸ºä¸ä¸ªåå«ææå½åæéå°çè¡ç»æçåè¡¨ file.softspace ä¸º0è¡¨ç¤ºå¨è¾åºä¸æ°æ®åï¼è¦å ä¸ä¸ä¸ªç©ºæ ¼ç¬¦ï¼1è¡¨ç¤ºä¸å ãè¿ä¸ªå±æ§ä¸è¬ç¨åºåç¨ä¸çï¼ç±ç¨åºåé¨ä½¿ç¨
{% endhint %}

## åèæç®

* [https://blog.csdn.net/weixin\_44041700/article/details/108173614](https://blog.csdn.net/weixin\_44041700/article/details/108173614)
* [https://www.cnblogs.com/soymilk2019/p/14442806.html?ivk\_sa=1024320u](https://www.cnblogs.com/soymilk2019/p/14442806.html?ivk\_sa=1024320u)
* [https://www.cnblogs.com/ymjyqsx/p/6554817.html](https://www.cnblogs.com/ymjyqsx/p/6554817.html)
* [https://blog.csdn.net/MsSpark/article/details/86745391](https://blog.csdn.net/MsSpark/article/details/86745391)
