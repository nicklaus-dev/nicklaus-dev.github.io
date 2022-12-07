---
layout: post
title: Introduce golang generics
date: 2022-12-07 16:50 +0800
categories: [golang]
tags: [golang]
---
# What is generics syntax sugar in go

> Let's look at the following code snippet.

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

# When to use generics in go

# Some tips of using generics
