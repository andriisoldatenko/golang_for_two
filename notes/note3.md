🔨Note #3 Настраиваем дебаггер

Сегодня наткнулся на то, что print в режиме дебаггера, не показывает длинные строки.


> main.main() ./main.go:7 (PC: 0x10b08d4)
     2:
     3:	import "fmt"
     4:
     5:	func main() {
     6:		v1 := "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
=>   7:		fmt.Println(v1)
     8:	}
(dlv) p v1
"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa...+2 more"

(dlv) config -list
aliases            map[]
substitute-path    []
max-string-len     <not defined>
max-array-values   <not defined>
show-location-expr false
(dlv) config max-string-len 1000
(dlv) p v1
"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"

✅ Теперь можно напечатать длинную строку.