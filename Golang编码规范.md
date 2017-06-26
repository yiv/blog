# Golang 编码规范
## 1 注释
推荐使用C语言风格的 "//" 注释。注释必须是完整的句子，尽量简明，以句点结尾。
程序中每一个可被导出的（大写的）类型，都应该有一个注释。

**在编码阶段同步写好变量，函数，包注释，就可以使用godoc生成文档**

### 1.1 包注释
每个包都应该有一个包注释，包如果有多个go文件，就只需要在入口文件写包注释。
概况以 Package 开头。
```go
// Copyright 2009 The Go Authors. All rights reserved.
// Use of this source code is Governed by a BSD-style
// license that can be found in the LICENSE file.

// Package strings implements simple functions to manipulate strings.
package strings
```
### 1.2 函数和结构体注释
第一行写概况，并且使用被声明的名字作为开头。
```go
// Compile parses a regular expression and returns, if successful, a Regexp
// object that can be used to match against text.
func Compile(str string) (regexp *Regexp, err error) {

// Request represents a request to run a command.
type Request struct { ...
```
### 1.3 变量注释
In Go, exported field or function denoted by capital letter on the first letter, and it must have comment.
  
For field (on struct, var, or const) the recommended comment format is by using define verb after variable name.
  
For example,
```
// DefPort define the default port to listen on ...
var DefPort = 9002
```

## 2 变量名
### Use short variable names if possible
Common short variable names,
* x, y, and z for looping. Not i, j, etc. because its prone to error, let more than three deeps looping (which is a signal for bad programming) and not easy for quick reading.
* err for error variable
* ctx for context
* req for client request
* res for server response
* msg for general message input/output  

Common prefix for variable or function,
* jXXX for message in JSON struct
* bXXX for message in slice of bytes ([]byte)
* DefXXX or defXXX for default variable/constanta
### 2.1 包名
包名应该为小写单词，不要使用下划线或者混合大小写。
### 2.2 变量
* 全局变量：驼峰式，可导出的使用大写字母开头
* 参数传递：驼峰式，小写字母开头
* 局部变量：下划线风格命名 
### 2.3 接口
单函数的接口用 函数+"er" 命名，如：Reader，Writer
```
type Reader interface {
        Read(p []byte) (n int, err error)
}
```
2个函数的接口，组合命名
```
type WriteFlusher interface {
    Write([]byte) (int, error)
    Flush() error
}
```
3个以上函数的接口名，类似于结构体名
```
type Car interface {
    Start([]byte)
    Stop() error
    Recover()
}
```
## 3 import
对标准包，程序内部包，第三方包进行分组。
```
import (
    // This is system packages
    "os"
    "net/http"
    ...

    // Empty line followed by third party packages
    "third/party/library"
    ...

    // Empty line followed by our packages
    "github.com/yourcompany/yourlib"
    ...
)
```
引用包时不要使用相对路径。
```
// 这是不好的导入
import "../net"

// 这是正确的做法
import "github.com/repo/proj/src/net"
```
## 4 代码结构
### 4.1 Structure code as in Godoc layout
If you looks at the Go doc layout, each sections is ordered by the following format,
* package description
* package constanta
* package global variables
* package global functionstype
* type's methods

Builtin functions, like init(), main(), and TestMain() must be at the bottom of the source code. As an example see net package.
  
**Rationale**: Following go doc format will make code easy to read, because we know where each of section is located.
### 4.2 Package must have a file with the same name
Package named mypkg must have source file with the name mypkg.go. This file will be used to declare global variables, constanta, and init() function.

**Rationale**: easy to search where global variables, constanta, and init() defined.
### 4.3 One type (struct/interface) per file
The filename must follow the name of the type. For example, package X have two exported structs: Y and Z. So, in the directory X there would be two files: Y.go and Z.go.  

**Rationale:**
* Easy to search where type is defined
* Modularization by files
### 4.4 Package that create binary should be in "./cmd" directory
One of the things that I learned later in software development was when writing code, pretend that your code will be used by other developers, which means, write library first, program later. This is a mistake that we have been taught since college, because we learn to write program not library.

Go, in their subtle way, embrace this kind of thinking when developing software.


