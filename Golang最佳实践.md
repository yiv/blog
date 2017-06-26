# golang最佳编程实践总结

### init函数
* 不要在init做配置文件初始化

### main全局变量
* 所有配置文件在main中初始化

### 包全局变量
* 避使用包全局变量，使用依赖注入（更好的是同时使用接口+闭包），参考[Practical Persistence in Go: Organising Database Access](http://www.alexedwards.net/blog/organising-database-access)

### Golang编码规范
* https://segmentfault.com/a/1190000000464394

### 程序配置
* 用参数，不用配置文件，解少程序文件依赖