# มันคือความมัน
เราจะมาลองทำความเข้าใจเกี่ยวกับการใช้งาน module 
โดยตอนนี้จากที่ได้ลองเล่นมาสัก 2-3 HR สามารถสรุปได้ตามความเข้าใขของผมเองได้ประมาณว่า
- เราสามารถสร้าง Project GO โดยที่ไม่จำเป็นต้องอยู่ใน ``GOPATH`` ได้แล้ว
- การ Import module อื่นๆหรือของชาวบ้านมาใช้สามารถทำได้ 2 วิธี
    - การที่เราใช้ Command go get github.com/owner-name/module-
    - การที่เราใช้ วิธี replace location ไปที directory ที่ module นั้นเก็บอยู่ในเครื่องเรา

### Question
โดยเราจะทำการทดสอบ สร้าง project go เป็น 3 project 
```
<go-projects>/
 |-- updev/
 |-- localService/
 |-- publicService/
```
โดย updev จะเป็น main application โดยจะมีการเรียกใช้งาน service จากทั้ง localService และ publicService โดย localService จะ run อยู่ที่ local แต่ publicService จะอยู่บน github

### ลุยๆๆๆ

- สร้าง updev project
    ```powershell
    PS D:\git-myself\GO\go-projects> mkdir updev
    PS D:\git-myself\GO\go-projects\updev> go mod init puza.com/updev
    go: creating new go.mod: module puza.com/updev
    ```
- create file ``updev.go`` ใน project updev
    ``` GO
    package main

    func main() {
        //TODO Request localService
        //TODO Request publicService
    }
    ```
- สร้าง publicService project โดยเริ่มต้นเราจะต้องไปสร้าง git repository ``https://github.com/ksupdev/publicService.git``
- ทำการสร้าง folder project publicService and setup git
    ```powershell
    PS D:\git-myself\GO\go-projects> mkdir publicService
    PS D:\git-myself\GO\go-projects\publicService> git init
    Initialized empty Git repository in D:/git-myself/GO/go-projects/publicService/.git/
    PS D:\git-myself\GO\go-projects\publicService> git remote add origin https://github.com/ksupdev/publicService.git


    PS D:\git-myself\GO\go-projects\publicService> go mod init github.com/ksupdev/publicService
    go: creating new go.mod: module github.com/ksupdev/publicService


    ```

- ทำการสร้าง file ``publicService.go`` in project publicService
    ```go
    [filename : publicService.go]
    package publicService

    import "fmt"

    func SayHi(name string) string {
        message := fmt.Sprintf("Hi, %v. My name's publicService", name)
        return message
    }
    ```

- ทำการ Commit and push project to Github
    ```powershell
    PS D:\git-myself\GO\go-projects\publicService> git add .
    PS D:\git-myself\GO\go-projects\publicService> git commit -m "setup project"
    PS D:\git-myself\GO\go-projects\publicService> git push -u origin master


    PS D:\git-myself\GO\go-projects\publicService> git tag v1.0.0
    PS D:\git-myself\GO\go-projects\publicService> git push --tags
    Total 0 (delta 0), reused 0 (delta 0)
    To https://github.com/ksupdev/publicService.git
    * [new tag]         v1.0.0 -> v1.0.0
    ```

- ตอนนี้เรามาเริ่มจากการ เรียกใช้งาน module ที่อยู่บน Github โดยเราจะเริ่มต้นจากการแก้ไข ``updev.go``

    ```go
    [filename : updev.go]
    package main

    import (
        "fmt"
        "github.com/ksupdev/publicService"
    )


    func main() {
        //TODO Request localService
        //TODO Request publicService
        message := publicService.SayHi("Gladys")
        fmt.Println(message)
    }

    ```
    และทำการ ทดสอบ ``go run updev.go``

    ```powershell
    PS D:\git-myself\GO\go-projects\updev> go run updev.go
    updev.go:5:2: no required module provides package github.com/ksupdev/publicService; to add it:
            go get github.com/ksupdev/publicService
    ```

    หลังจากที่ ลองทำการ run เราจะพบว่า Go ได้ทำการแจ้งให้ทำการ run ``go get github.com/ksupdev/publicService`` 

    ```powershell
    PS D:\git-myself\GO\go-projects\updev> go get github.com/ksupdev/publicService
    go: downloading github.com/ksupdev/publicService v1.0.0
    go get: added github.com/ksupdev/publicService v1.0.0
    PS D:\git-myself\GO\go-projects\updev> go run updev.go
    Hi, Gladys. My name's publicService
    ```
    ได้ผลลัพธ์ตามที่เราต้องการ

- ทำการสร้าง Project localService
    ```powershell
    PS D:\git-myself\GO\go-projects> mkdir localService
    PS D:\git-myself\GO\go-projects\localService> go mod init puza.com/localService
    go: creating new go.mod: module puza.com/localService
    ```
- Create file ``localService.go``
    ```go
    package localService

    import "fmt"

    func SayHi(name string) string {
        message := fmt.Sprintf("Hi, %v. My name's localService", name)
        return message
    }
    ```

- ทำการแก้ไข ``updev.go``
    ``` go
    package main

    import (
        "fmt"
        "github.com/ksupdev/publicService"
        "puza.com/localService"
    )


    func main() {
        //TODO Request localService
        message1 := localService.SayHi("Gladys")
        fmt.Println(message1)
        //TODO Request publicService
        message := publicService.SayHi("Gladys")
        fmt.Println(message)
    }
    ```
- ลองทำการ run ทดสอบ
    ```powershell
    PS D:\git-myself\GO\go-projects\updev> go run updev.go
    updev.go:6:2: no required module provides package puza.com/localService; to add it:
            go get puza.com/localService
    ```
    จะเห็นว่ามีการแนะนำให้ใช้คำสั่ง ``go get puza.com/localService`` แต่ก็จะไม่เกิดอะไรขึ้นเนื่องจากเราไม่ได้ทำการ public อะไรขึ้นไป

    ทำการ run command `` go mod edit -replace=puza.com/localService=../localService``

    ```powershell
    PS D:\git-myself\GO\go-projects\updev> go mod edit -replace=puza.com/localService=../localService
    go mod: -replace=puza: need old[@v]=new[@w] (missing =)
    ```

    และทำการเพิ่ม ``replace puza.com/localService => ../localService`` ใน file ``go.mod`` ใน Project updev

    แลทำการ run ``go mod tidy``
    ``` powershell
    PS D:\git-myself\GO\go-projects\updev> go mod tidy
    go: found puza.com/localService in puza.com/localService v0.0.0-00010101000000-000000000000
    ```
    และทำการ test run อีกที
    ``` powershell
    PS D:\git-myself\GO\go-projects\updev> go run updev.go
    Hi, Gladys. My name's localService
    Hi, Gladys. My name's publicService
    ```
    
    โดยในส่วนของ file updev/go.mod เราจะมีการ เพิ่ม ``replace puza.com/localService => ../localService`` แค่ส่วนเดียว ที่เหลือเป็นการ auto gen ทั้งหมด
    ```go
    module puza.com/updev

    go 1.16

    require (
        github.com/ksupdev/publicService v1.0.0
        puza.com/localService v0.0.0-00010101000000-000000000000
    )

    replace puza.com/localService => ../localService


    ```


### command remind

``` powershell

```