## sync的包

一个很好用的用于线程之间同步的类就是WaitGroup。

WaitGroup总共有三个方法：
* `Add(delta int)`

    添加或者减少等待goroutine的数量

* `Done()`

    相当于Add(-1)

* `Wait()`

    执行阻塞，直到所有的WaitGroup数量变成0



```go
package main

import (
	"fmt"
	"sync"
	"time"
	
)

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 5; i = i + 1 {
        wg.Add(1)
        // 添加主线程需要等待的事务数量
        go func(n int) {
            // defer wg.Done()
            defer wg.Add(-1)
            // 上面两种写法等价
            //defer关键字，表示其后的语句是函数最后执行的语句
		    
            EchoNumber(n)
        }(i)
    }

    wg.Wait()
    // 主线程等待waitgroup中所有子线程执行完毕。
}

func EchoNumber(i int) {
    time.Sleep(3e9)
    fmt.Println(i)
}
```

上述程序执行结果

```
0
1
2
3
4
5
```

---

## `sync.atomic`包

上述多线程并发的方法在实际应用中还要配合`sync`包中的`atomic`包

一个典型场景为在匿名函数（子线程）中修改主线程拥有的变量，这时就需要对变量进行atomic操作，从而确保变量有准确的值！atomic方法就是在操作变量之前加一个mutex锁，操作结束之后去掉mutex锁，这就是实现了操作的原子性！


```go
package main

import (
	"fmt"
	"sync"
	"time"
	"sync/atomic"
)

func main() {
    var wg sync.WaitGroup
    count := 10
    for i := 0; i < 5; i = i + 1 {
	    wg.Add(1)
	    // 添加主线程需要等待的事务数量
	    go func(n int) {
	        defer wg.done()
	        EchoNumber(n)
	        atomic.AddUint64(&count, uint64(1))
	    }(i)
    }
    wg.Wait()
}

func EchoNumber(i int) {
    time.Sleep(3e9)
    fmt.Println(i)
}
```

----

## 总结

通过多线程并发的方式可以利用充分利用服务器的计算资源，达到快速响应的目的！又学到一招！

