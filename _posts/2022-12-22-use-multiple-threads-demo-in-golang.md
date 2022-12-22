---
layout: post
title: Use Multiple Threads Demo in Golang
date: 2022-12-22 22:33 +0800
---
# Golang Demo Snippets

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func main() {
	n := 1000
	wg := sync.WaitGroup{}
	wg.Add(n)
	start := time.Now()
	for i := 0; i < n; i++ {
		go func() {
			defer wg.Done()
			handle()
		}()
	}
	wg.Wait()
	end := time.Now()
	cost := end.Sub(start).Milliseconds()
	fmt.Printf("cost: %v\n", cost)
}

func handle() {
	time.Sleep(time.Second)
}
```
