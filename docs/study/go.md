#### Go环境变量与工作目录

```tex
根据约定，GOPATH下需要建立3个目录：
bin（存放编译后生成的可执行文件）
pkg（存放编译后生成的包文件）
src（存放项目源码）
```

#### Go常用命令简介

```tex
go get：获取远程包（需 提前安装 git或hg）
go run：直接运行程序
go build：测试编译，检查是否有编译错误
go fmt：格式化源码（部分IDE在保存时自动调用）
go install：编译包文件并编译整个程序
go test：运行测试文件
go doc：查看文档
```

#### 外部包和私有包的区别
```tex
函数名大写的就是外部包，比如fmt.Println()
函数名小写的就是私有包，比如fmt.Println()

```

#### 
