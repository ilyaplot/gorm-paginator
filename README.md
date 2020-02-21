# gorm-paginator

## Usage

```bash
go get github.com/ilyaplot/gorm-paginator/pagination
```

```go
type User struct {
	ID       int
	UserName string `gorm:"not null;size:100;unique"`
}

var users []User
db = db.Where("id > ?", 0)

_, err := pagination.Pagging(&pagination.Param{
    DB:      db,
    Page:    1,
    Limit:   3,
    OrderBy: []string{"id desc"},
}, &users)

if err != nil {
  panic(err)
}
```

## With Gin

```go
r := gin.Default()
r.GET("/", func(c *gin.Context) {
    page, _ := strconv.Atoi(c.DefaultQuery("page", "1"))
    limit, _ := strconv.Atoi(c.DefaultQuery("limit", "3"))
    var users []User

    paginator, err := pagination.Pagging(&pagination.Param{
        DB:      db,
        Page:    page,
        Limit:   limit,
        OrderBy: []string{"id desc"},
        ShowSQL: true,
    }, &users)
    
    if err != nil {
      panic(err)
    }

    c.JSON(200, paginator)
})
```

## License

[MIT](LICENSE)