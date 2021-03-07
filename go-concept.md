### What's PACAKGE
นิยามสั่นๆ 
```
Package คือการที่เรานำ Program ของเราไป pack รวมกันเพื่อให้ผู้อื่นสามารถใช้งานได้
```


> Go compiler จะ ทำการ Run file ที่มีการระบุ ``package main``
```go
// file main.go
package main

func main(){
	println("Hello World")
}
```
เราลอง สร้าง ``updev_go_school.go``
```go
package updev_go_school

SchoolName := "เตรียมอุดมศึกษา";
func GetSchoolAddress() string {
    return "กรุงเทพ";
}

```

และทำการ run 
```
D:\git-myself\GO\go-workspace\src\ananda-azuredevops.com\updev_go_school> go run .\updev_go_school.go
go run: cannot run non-main package
```
ซึ่งใน Error ก็มีการบอกอย่างชัดเจน

ทำการ run ``go mod init`` เพื่อทำการสร้าง module
```go
module ananda-azuredevops.com/updev_go_school
go 1.16
```


## Package Import
> การ Import package มีด้วยกัน 2 ประเภทคือ 
- GOROOT : Package standard ของ GO
- GOPATH : นั้นคือ ชุด Package ที่มีการพัฒนาไม่ว่าจากเราพัฒนาเอง หรือ Community แค่ที่สำคัญที่สุดก็คือ pakcage เหล่านั้นจะต้องมีการ Download มาไว้ใน GOPATH แล้วเท่านั้น ภาษา Go ถึงจะสามารถ Compile ได้




# Go module

Create Module

```powershell
PS D:\git-myself\GO\go-projects\nummanip> git init
Initialized empty Git repository in D:/git-myself/GO/go-projects/nummanip/.git/
PS D:\git-myself\GO\go-projects\nummanip> git remote add origin https://github.com/ksupdev/nummanip.git
PS D:\git-myself\GO\go-projects\nummanip> go mod init github.com/ksupdev/nummanip
go: creating new go.mod: module github.com/ksupdev/nummanip
```

# Noted from Go
[ref](https://golang.org/doc/tutorial/create-module)

>The go mod init command creates a go.mod file to track your code's dependencies. So far, the file includes only the name of your module and the Go version your code supports. 

```powershell

PS D:\git-myself\GO\go-projects> mkdir greetings
PS D:\git-myself\GO\go-projects\greetings> go mod init example.com/greetings
go: creating new go.mod: module example.com/greetings
```

create new file in ``\greetings``

``` go
[filename : greetings.go]

package greetings

import "fmt"

// Hello returns a greeting for the named person.
func Hello(name string) string {
    // Return a greeting that embeds the name in a message.
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message
}
```

> the := operator is a shortcut for declaring and initializing a variable in one line

## Call your code from another module

```
<home>/
 |-- greetings/
 |-- hello/
```

> For production use, you’d publish the example.com/greetings module from its repository (with a module path that reflected its published location), where Go tools could find it to download it. For now, because you haven't published the module yet, you need to adapt the example.com/hello module so it can find the example.com/greetings code on your local file system.


```powershell
PS D:\git-myself\GO\go-projects> mkdir hello
PS D:\git-myself\GO\go-projects\hello> go mod init example.com/hello
go: creating new go.mod: module example.com/hello

```

``` Go
[filename : hello.go]
package main

import (
    "fmt"

    "example.com/greetings"
)

func main() {
    // Get a greeting message and print it.
    message := greetings.Hello("Gladys")
    fmt.Println(message)
}
```

เนื่องจากในกรณีเราต้องการจะทดสอบการเรียกใช้ module ที่เราพัฒนาขึ้นเองและไม่ได้ public ไปข้างนอด

>To do that, use the go mod edit command to edit the example.com/hello module to redirect Go tools from its module path (where the module isn't) to the local directory (where it is).

- ไปที่ Directory ``\hello`` และทำการ run command ``go mod edit -replace=example.com/greetings=../greetings`` 

    ```powershell
    PS D:\git-myself\GO\go-projects\hello> go mod edit -replace=example.com/greetings=../greetings
    go mod: -replace=example: need old[@v]=new[@w] (missing =)

    PS D:\git-myself\GO\go-projects\hello> go mod tidy
    go: found example.com/greetings in example.com/greetings v0.0.0-00010101000000-000000000000
    ```
- ทำการ ทดสอบ
    ```
    PS D:\git-myself\GO\go-projects\hello> go run .\hello.go
    Hi, Gladys. Welcome! v2
    ```