---
cover: >-
  https://images.unsplash.com/photo-1515275989527-65f0b4bdb22c?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxfHxzdHJ1Y3R8ZW58MHx8fHwxNjQ4MjA4NTkx&ixlib=rb-1.2.1&q=85
coverY: 0
---

# ð³ ç»æä½ååæ°ä¼ é

## çè®º

**é¦åç»æä½åå½æ°åæ°æä¸ç§ä¼ éæ¹å¼:**

ä¸æ¯ä¼ éç»æä½åéï¼è¿æ¯å¼ä¼ éï¼äºæ¯ä¼ éç»æä½æéï¼è¿æ¯å°åä¼ éï¼ä¸æ¯ä¼ éç»æä½æåï¼å½ç¶è¿ä¹åä¸ºå¼ä¼ éåå°åä¼ éã

ä»¥ä¼ å¼ç¨è°ç¨æ¹å¼ä¼ éç»ææ¯ç¨ä¼ å¼æ¹å¼ä¼ éç»ææçé«ãä»¥ä¼ å¼æ¹å¼ä¼ éç»æéè¦å¯¹æ´ä¸ªç»æåä¸ä»½æ·è´ã

## ä¾å­

#### 1.ä¸é¢ä¼ éç»æä½åé

```cpp
#include<stdio.h>
#include<string.h>
#define format "%d\n%s\n%f\n%f\n%f\n"
struct student
{
    int num;
    char name[20];
    float score[3];
};
void change( struct student stu );
int main()
{
    
    struct student stu;
    stu.num = 12345;
    strcpy(stu.name, "Tom");
    stu.score[0] = 67.5;
    stu.score[1] = 89;
    stu.score[2] = 78.6;
    change(stu);
    printf(format, stu.num, stu.name, stu.score[0], stu.score[1],stu.score[2]);
    printf("\n");
    return 0;
}
 
void change(struct student stu)
{
    stu.score[0] = 100;
    strcpy(stu.name, "jerry");
}
```

![](../../.gitbook/assets/20140815150142953.jpeg)

å¯ä»¥çå°æç»è¾åºçscoreå¼å¹¶æªæ¹å

#### 2.å°åä¼ é

```cpp
#include<stdio.h>
#define format "%d\n%s\n%f\n%f\n%f\n"
struct student
{
    int num;
    char name[20];
    float score[3];
};
void change( struct student* stu );
int main()
{
    
    struct student stu;
    stu.num = 12345;
    strcpy(stu.name, "Tom");
    stu.score[0] = 67.5;
    stu.score[1] = 89;
    stu.score[2] = 78.6;
    change(&stu);
    printf(format, stu.num, stu.name, stu.score[0], stu.score[1],stu.score[2]);
    printf("\n");
    return 0;
}
 
void change(struct student* p)
{
    p->score[0] = 100;
    strcpy(p->name, "jerry");
}
```

![](../../.gitbook/assets/20140815151805750.jpeg)

å¯ä»¥çå°ï¼éè¿å°åä¼ éä¿®æ¹äºç»æä½åçæ°æ®

ç¨\&stuåå®å,\&stuæ¯ç»æä½åéstuçå°åãå¨è°ç¨å½æ°æ¶å°è¯¥å°åä¼ éç»å½¢åp(pæ¯æéåé)ãè¿æ ·på°±æåstuã

å¨changeå½æ°ä¸­æ¹åç»æä½åæåçå¼ï¼å¨ä¸»å½æ°ä¸­å°±è¾åºäºæ¹ååçå¼

#### 3.ç»æä½æåçå°åä¼ éåå¼ä¼ é

è¿ä¸ªç±»ä¼¼äºåä¸åéçä¼ éï¼è¿éä¹æ²¡å¿è¦è¯´äºï¼å½ç¶æ¯å°åä¼ éæè½ä¿®æ¹ã

æä¸ä¸ªå®æ´çç»æä½åéä½ä¸ºåæ°ä¼ é,è¦å°å¨é¨æåå¼ä¸ä¸ªä¸ä¸ªä¼ é,è´¹æ¶é´åè´¹ç©ºé´,å¼éå¤§ãå¦æç»æä½ç±»åä¸­çæåå¾å¤ï¼ææä¸äºæåæ¯æ°ç»ï¼åç¨åºè¿è¡æçä¼å¤§å¤§éä½ãå¨è¿ç§æåµä¸ï¼ç¨æéåå½æ°åæ°æ¯è¾å¥½ï¼è½æé«è¿è¡æçã
