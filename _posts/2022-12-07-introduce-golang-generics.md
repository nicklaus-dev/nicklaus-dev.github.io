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

This is called `constraint` in go 1.18, it can limit a set of types.

```go
type Number interface {
	int | int32 | int64
}
```

And the `function` or `struct` type can use `[T number]` to define this `constraint`, then use it in the function context.

# When to use generics in go

## A series of same logic with different types

> eg. map-reduce operations, like filter

```go
func Filter[T any](list []T, filterFn func(value T) bool) []T {
	var result []T
	for _, v := range list {
		if filterFn(v) {
			result = append(result, v)
		}
	}

	return result
}
```

## Different data types with same interface

```go
package main

import (
	"fmt"
)

type Error interface{
	Msg() string
}

type User struct {
	Name string
	Age int
}

func (u User) Msg() string {
	return "invalid user name"
}

type Car struct {
	Brand string
}


func (c Car) Msg() string {
	return "invalid brand"
}

func checkMsg[E Error](e E) E {
	if e.Msg() == "invalid brand" {
		return e
	}

	// return empty e
	return *new(E)
}

func main() {
	u := User{
		Name: "nick",
	}
	c := Car{
		Brand: "benz",
	}
	fmt.Printf("before u: %v\n", u) // before u: {nick 0}
	fmt.Printf("before c: %v\n", c) // before c: {benz}
	u = checkMsg(u)
	c = checkMsg(c)
	fmt.Printf("after u: %v\n", u) // after u: { 0}
	fmt.Printf("after c: %v\n", c) // after c: {benz}
}

```

# Some tips of using generics

- Don't use generics for practice generics syntax, but for erasing duplicated code.
- Don't define `func` type with generics, it don't work! Because generics will be compiled to real type in compiling.
- Don't use generics in define `interface`.
