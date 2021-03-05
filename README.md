# GO go
This project use for keep example and labs

## Setup GO go
- [Downdload GO](https://golang.org/doc/install)
- Check version after install go 
```
C:\Users\updev> go version
go version go1.16 windows/amd64
```

## Setup GO work space

### windows (10)
- run ``go env`` เพื่อเป็นการตรวจสอบ go environment ของเครื่องเรา
    ```
    PS D:\git-myself\GO> go env
    set GO111MODULE=
    set GOARCH=amd64
    set GOBIN=
    set GOCACHE=C:\Users\karoon\AppData\Local\go-build
    set GOENV=C:\Users\karoon\AppData\Roaming\go\env
    set GOEXE=.exe
    set GOFLAGS=
    set GOHOSTARCH=amd64
    set GOHOSTOS=windows
    set GOMODCACHE=C:\Users\karoon\AppData\workspace\pkg\mod
    set GOPATH=C:\Users\karoon\AppData\workspace
    ```
    โดยค่าที่แสดงจะเป็นค่า Default ของแต่ละ เครื่อง
- Create my workspace สำหรับเก็บ project ของเรา
    ```
    PS D:\git-myself\GO> mkdir go-workspace
    Directory: D:\git-myself\GO
    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    d-----       05/03/2021     13:18                go-workspace
    ```
- ทำการ Config GOPATH ใน windows environment variable

  [windoeswnv](go-windows-env.png)



## Example project
- [Hello word](updev-go-hellowold) : Hello world

