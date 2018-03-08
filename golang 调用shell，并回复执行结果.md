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
**NOTE:闭包里的非传递参数外部变量值是传引用的**
