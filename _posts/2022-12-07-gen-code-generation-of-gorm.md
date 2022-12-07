---
layout: post
title: Gen -- code generation of GORM
date: 2022-12-07 18:37 +0800
category: [golang]
tag: [golang, tools]
---
# What can Gen do

## Database model generation
1.**Install**
```shell
go install gorm.io/gen/tools/gentool@latest
```
2.**Usage**
```shell
gentool -dsn "user:pwd@tcp(localhost:3306)/database?charset=utf8mb4&parseTime=True&loc=Local" -tables "orders,doctor"
```

## Basic CRUD method generation

> Run following code with `go run main.go`

```go
package main

import (
  "gorm.io/gen"
  "gorm.io/gorm"
  "gorm.io/driver/sqlite"
)

func main() {
  // Initialize the generator with configuration
  g := gen.NewGenerator(gen.Config{
     OutPath: "../dal", // output directory, default value is ./query
     Mode:    gen.WithDefaultQuery | gen.WithQueryInterface,
     FieldNullable: true,
  })

  // Initialize a *gorm.DB instance
  db, err := gorm.Open(sqlite.Open("test.db"), &gorm.Config{})

  // Use the above `*gorm.DB` instance to initialize the generator,
  // which is required to generate structs from db when using `GenerateModel/GenerateModelAs`
  g.UseDB(db)

  // Generate default DAO interface for those specified structs
  g.ApplyBasic(model.Customer{}, model.CreditCard{}, model.Bank{}, model.Passport{})

  // Generate default DAO interface for those generated structs from database
  companyGenerator := g.GenerateModelAs("company", "MyCompany"),
  g.ApplyBasic(
    g.GenerateModel("users"),
    companyGenerator,
    g.GenerateModelAs("people", "Person",
      gen.FieldIgnore("deleted_at"),
      gen.FieldNewTag("age", `json:"-"`),
    ),
  )

  // Execute the generator
  g.Execute()
}
```

## SQL code generation
Write target format comment on the top of method in one `inteface`

```go
type Querier interface {
  // SELECT * FROM @@table WHERE id=@id
  GetByID(id int) (gen.T, error) // GetByID query data by id and return it as *struct*

  // GetUsersByRole query data by roles and return it as *slice of pointer*
  //   (The below blank line is required to comment for the generated method)
  //
  // SELECT * FROM @@table WHERE role IN @rolesName
  GetByRoles(rolesName ...string) ([]*gen.T, error)

  // InsertValue insert value
  //
  // INSERT INTO @@table (name, age) VALUES (@name, @age)
  InsertValue(name string, age int) error
}

g := gen.NewGenerator(gen.Config{
  // ... some config
})

// Apply the interface to existing `User` and generated `Employee`
g.ApplyInterface(func(Querier) {}, model.User{}, g.GenerateModel("employee"))

g.Execute()
```

# Some tips of using Gen & Gorm

1. The generated code shouldn't be modified.
2. Use [cobra](https://github.com/spf13/cobra) to enhance the `genTool`.
3. Initialize the Query variable golbally.
