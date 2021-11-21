## 堆/栈空间

堆与栈的比较：

申请方式

栈（stack) : 由系统自动分配

堆（heap) : 需程序员自己申请

## int * 指针当数组使用

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int nums[] = { 2, 3, 6, 7, 9, 1 };
    int target = 10;
    int *result = NULL; // 定了一个 int * 指针
    for(int i=0; i<6; i++)
    {
        for(int j=0; j<6; j++){
            if(nums[i] + nums[j] == target)
            {
                result = (int*)malloc(sizeof(int)*2);
                result[0] = i;
                result[1] = j;
            }
        }
    }
    printf("result: %d\n", result[0]);
    printf("result: %d\n", result[1]);
    return 0;
}
```

