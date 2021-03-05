# updev-go-hellowold

## Step of work
- Create project directory with command ``mkdir updev-go-hellowold``
- You can run with command
    ```
    PS D:\git-myself\GO\updev-go\updev-go-hellowold> go run main.go
    Hello world
    ```
- Build source code for production 

    ```
    PS D:\git-myself\GO\updev-go\updev-go-hellowold> go build
    go: cannot find main module, but found .git/config in D:\git-myself\GO\updev-go
            to create a module there, run:
            cd .. && go mod init
    ```
    จากด้านบนเราจะเห็นว่าพอเรา ``go build`` ใน Directory project แต่ไม่สามารถทำได้ เนื่องจากในการ ที่จะทำการ build เราจำเป็นต้องทำการสร้าง file ``go.mod`` หรือก็คือ File package management เหมือน พวก pom.xml (maven) หรือ package.json (nodejs) เป็นต้น

    ดังนั้นเราจะเริ่มจากการสร้าง file โดยการใช้ command ``go mod init hello/main``

    ```
    PS D:\git-myself\GO\updev-go\updev-go-hellowold> go mod init hello/main
    go: creating new go.mod: module hello/main
    go: to add module requirements and sums:
        go mod tidy
    PS D:\git-myself\GO\updev-go\updev-go-hellowold> ls 

    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    -a----       05/03/2021     12:45             27 go.mod
    -a----       05/03/2021     12:32             59 main.go
    -a----       05/03/2021     11:13            291 README.md
    ```
    หลังจากนั้นลองทำการ Build อีกที

    ```
    PS D:\git-myself\GO\updev-go\updev-go-hellowold> go build
    PS D:\git-myself\GO\updev-go\updev-go-hellowold> ls

    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    -a----       05/03/2021     12:46        1227264 main.exe
    .....
    ....

    ```
    และเมื่อเราได้ ไฟล์ main.exe เราก็จะสามารถทำการ run เพื่อทำงานของเราได้ทันที

    ``` 
    PS D:\git-myself\GO\updev-go\updev-go-hellowold> .\main.exe
    Hello world
    ```
