
UDPServer通过gudp包提供支持，以下是方法列表：
```go
func GetServer(names ...string) *Server
func NewServer(address string, handler func(*net.UDPConn), names ...string) *Server
func (s *Server) Run() error
func (s *Server) SetAddress(address string)
func (s *Server) SetHandler(handler func(*net.UDPConn))
```

来一个简单的示例：
gitee.com/johng/gf/blob/master/geg/net/udp_server.go
```go
package main

import (
    "fmt"
    "net"
    "gitee.com/johng/gf/g/net/gudp"
)

func main() {
    gudp.NewServer(":8999", func(conn *net.UDPConn) {
        buffer := make([]byte, 1024)
        if length, addr, err := conn.ReadFromUDP(buffer); err == nil {
            fmt.Println(string(buffer[0 : length]), "from", addr.String())
        }
    }).Run()
}
```

UDPServer是阻塞运行的，用户可以在自定义的回调函数中根据读取内容进行并发处理。

使用以下命令向服务端发送UDP数据进行测试，随后查看服务端端是否有输出：

	echo "hello" > /dev/udp/127.0.0.1/8999