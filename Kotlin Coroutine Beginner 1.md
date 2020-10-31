###### 协程的基本要素

- 挂起函数：以 suspend 修饰的函数
- 挂起函数只能在其他挂起函数或协程中调用
- 挂起函数调用时包含了协程挂起的语义
- 挂起函数返回时则包含了协程恢复的语义
- Continuation
- 挂起函数的类型，调用挂起函数需要传入 Continuation
- 将回调转写成挂起函数
  - 使用 suspendCoroutine 获取挂起函数的 Continuation
  - 回调成功的分支使用 Continuation.resume(value)
  - 回调失败则使用 Continuation.resumeWithException(e)
- 协程的创建
  - 需要一个函数：suspend function 
  - 也需要一个 API：createConroutine, startCoroutine
- 协程上下文
  - 协程执行过程中需要携带数据
- 拦截器
  - 拦截器 ContinuationInterceptor 是一类协程上下文元素
  - 可以对协程上下文所在协程的 Continuation 进行拦截

###### 官方协程框架的概念

- builders
  - launch：启动一个普通的协程
  - async：启动一个有结果返回的协程
  - runBlocking
- delay
- 调度器
  - 拦截器
  - Default
  - Android
- 取消响应
  - 协程支持取消，并将状态流转为取消状态
  - 协程内部的挂起函数调用支持对所在协程的取消状态的响应才能真正取消
- 异常处理
  - CoroutineExceptionHandler
  - launch 内出现未捕获的异常会抛给异常处理器
  - async 内出现未捕获的异常只有在 await 调用时才会抛出
  - 协程出现异常后都会根据所在作用域来尝试将异常向上传递
- 作用域
  - GlobalScope
    - 顶级作用域
    - 异常不向外部传播
  - coroutineScope
    - 协同作用域
    - 主要是用来获取当前挂起函数所在的作用域
    - 子协程异常后会取消父协程
  - supervisorScope
    - 主从作用域
    - 主要是用来向父协程屏蔽子协程的异常的
    - 子协程异常后不会取消父协程