
## 概述
- https://go.dev/
- https://go-zh.org/

## 入门指引

### 安装

https://go.dev/doc/install

### 配置环境变量

```Shell
# 工作目录
export GOPATH=$HOME/go
# golang 的安装路径
export GOROOT=/usr/local/go
# 存放go install生成的可执行文件
export GOBIN=$GOPATH/bin
export PATH="$GOBIN:$PATH"
```

### 本地安装Tour源码
```sh

```
### Hello

1. 创建项目
```Shell

mkdir hello
cd hello
go mod init example/hello
```

2. 编写hello.go
```Go
package main
import "fmt"
func main() {
	fmt.Println("Hello, World!")
}
```
3. 运行 
```Shell
go run .
```
4. 使用第三方库
```Shell
# 使用本地未发布包
go mod edit -replace example.com/greetings=../greetings

# 同步依赖库
go mod tidy
```
