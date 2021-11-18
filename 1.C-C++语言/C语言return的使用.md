## return 用在 void 函数中

```c
#include <stdio.h>


void demo(int a){
    if (a < 0) {
        printf("a 小于0\n");
    } else {
        printf("a 为正数\n");
        return; // 直接跳出函数
    }

    printf("测试能否执行到这里\n"); // 如果 a >= 0, 则这条语句不会被执行到
}

int main(void)
{
    demo(1);
    return 0;
}
```

