[![GoDoc](https://godoc.org/github.com/adlane/exec?status.png)](https://godoc.org/github.com/adlane/exec)

# exec
A golang package to interact with a process running in background

### Example

```go
package main

import (
        "fmt"
        "time"

        "github.com/adlane/exec"
)

func main() {
        ctx := exec.InteractiveExec("bash", "-i")
        r := reader{}
        go ctx.Receive(&r, 5*time.Second)
        ctx.Send("echo hello world\n")
        time.Sleep(time.Second)
        ctx.Send("ls\n")
        time.Sleep(time.Second)
}

type reader struct {
}

func (*reader) OnData(b []byte) bool {
        fmt.Print(string(b))
        return false
}

func (*reader) OnError(b []byte) bool {
        fmt.Print(string(b))
        return false
}

func (*reader) OnTimeout() {}
```

### Output

```bash
$ echo hello world
hello world
$ ls
bash.go
cat-random.go 
```
