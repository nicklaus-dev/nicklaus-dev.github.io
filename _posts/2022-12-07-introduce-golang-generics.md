---
layout: post
title: Introduce golang generics
date: 2022-12-07 16:50 +0800
categories: [golang, basics]
tags: [golang]
---
# What is generics syntax sugar in golang?

> Look at following code snippet

```go
type Number interface {
	int | int32 | int64
}

func sum[T Number](nums []T) T {
	var result T
	for _, num := nums {
		result += num
	}
	return result
}
```
