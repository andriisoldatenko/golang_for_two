Note #6 Дебажим тесты

Итак, часто нужно запустить 1 тест да еще и в режиме отладки, например, когда вы написали тест который повторяет баг. Все очень просто (хотя из доков не особо очевидно): 

dlv test -- -test.run NameOfYourTest/PartOfTheName* (по сути тоже самое что и go test -run)

Или живой пример:

```
➜  debug_test dlv test -- -test.run TestFibonacciBig
(dlv) b main_test.go:6
Breakpoint 1 set at 0x115887f for github.com/andriisoldatenko/debug_test.TestFibonacciBig() ./main_test.go:6
(dlv) c
> github.com/andriisoldatenko/debug_test.TestFibonacciBig() ./main_test.go:6 (hits goroutine(17):1 total:1) (PC: 0x115887f)
     1:	package main
     2:
     3:	import "testing"
     4:
     5:	func TestFibonacciBig(t *testing.T) {
=>   6:		var want int64 = 55
     7:		got := FibonacciBig(10)
     8:		if got.Int64() != want {
     9:			t.Errorf("Invalid Fibonacci value for N: %d, got: %d, want: %d", 10, got.Int64(), want)
    10:		}
    11:	}
(dlv)
```

Также можно запустить с -v (помним о go test -v):

```
➜  debug_test dlv test -- -test.v -test.run TestFibonacciBig
(dlv) c
=== RUN   TestFibonacciBig
--- PASS: TestFibonacciBig (0.00s)
PASS
```
