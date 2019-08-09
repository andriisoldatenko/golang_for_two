Note #8 Ast T-shirt :)

Допустим вы решили себе напечатать футболку с куском кода который на Go, но hello-world это скучно. Вот вам идея, взять кусок синтаксического дерева из if err != nil. А если серьезно то всегда интересно посмотреть во что превратится ваша программа, когда компилятор ее парсит и строит ast.

В общем получится что-то типа такого:
```
55  .  1: *ast.IfStmt {
56  .  .  If: 5:2
57  .  .  Cond: *ast.BinaryExpr {
58  .  .  .  X: *ast.Ident {
59  .  .  .  .  NamePos: 5:5
60  .  .  .  .  Name: "err"
61  .  .  .  .  Obj: *(obj @ 38)
62  .  .  .  }
63  .  .  .  OpPos: 5:9
64  .  .  .  Op: !=
65  .  .  .  Y: *ast.Ident {
66  .  .  .  .  NamePos: 5:12
67  .  .  .  .  Name: "nil"
68  .  .  .  }
69  .  .  }
```

Если чуток почистить, то можно и на чашку вместить:

```
&ast.IfStmt{
  Cond: &ast.BinaryExpr{
    X:  &ast.Ident{Name: "err"},
    Op: token.NEQ,
    Y:  &ast.Ident{Name: "nil"},
  },
}
```

- поиграться https://play.golang.org/p/4LmU4nbLI0x 
- почитать https://golang.org/pkg/go/ast/#example_Print
