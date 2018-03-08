example:
golang程序中执行 df -lh ，并获取执行结果
```
package main

import (
	"fmt"
	//"os"
	"os/exec"
)

func main() {
	c := "df -lh"
	cmd := exec.Command("sh", "-c", c)
	//out, err := cmd.Output()
	out, _ := cmd.Output()
	fmt.Print(string(out))
}
```
