## malloc和calloc

* calloc

    * 头文件 

        `#include <stdlib.h>`
    * 原型

        `void* calloc (size_t num, size_t size);`
    * 作用

        ```
        // 动态分配内存并且初始化为全0
        // 和malloc的区别在于malloc不初始化
        ```
    * demo
    ```cpp
    #inlucde <stdlib.h>

    // calloc() 分配内存空间并初始化
    char *str1 = (char *)calloc(10, 2);
    // malloc() 分配内存空间并用 memset() 初始化
    char *str2 = (char *)malloc(20);
    memset(str2, 0, 20);
    ```

