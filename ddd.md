```
package main

import (
	"fmt"
	"time"
)

func message_origine(c chan struct{}) {
	//触发事件
	fmt.Println("send chan")
	c <- struct{}{}
}

func goroutine1() chan struct{} {
	c := make(chan struct{})
	fmt.Println("goroutine 1")
	go func() {
		<-c
		fmt.Println("goroutine1")
	}()
	return c
}

func goroutine2() chan struct{} {
	c := make(chan struct{})
	fmt.Println("goroutine 2")
	go func() {
		<-c
		fmt.Println("goroutine2")
	}()
	return c
}

func monitor(c chan struct{}, subs []chan struct{}) {
	go func() {
		for {
			<-c
			fmt.Println("getChan")
			fmt.Println(subs)
			for _, d := range subs {
				//通知其他协程
				fmt.Printf("%v\n", d)
				d <- struct{}{}
			}
		}
	}()
}

func main() {
	fmt.Println("the main goroutine")
	subs := make([]chan struct{}, 0)
	c := goroutine1()
	subs = append(subs, c)
	c = goroutine2()
	subs = append(subs, c)
	fmt.Println(subs)
	c = make(chan struct{})
	monitor(c, subs)
	//time.Sleep(3 * time.Second)
	message_origine(c)
	time.Sleep(3 * time.Second)
}
```
