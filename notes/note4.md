Note #4 Дебажим приложение на Go в докере 🐳

Итак нам понадобится:
- прямые руки и тазик с предустановленным докером

$ cat Dockerfile
```
FROM golang:1.13

WORKDIR /go/src/app
COPY . .

RUN go get -u github.com/go-delve/delve/cmd/dlv

CMD ["app"]
```

$ docker build -t my-golang-app .

# Это всего лишь один из вариантов, иногда нужно вместо bash сразу dlv запускать и так далее
$ docker run -it --rm my-golang-app bash

$ root@03c1977b1063:/go/src/app# dlv main.go
Error: unknown command "main.go" for "dlv"
Run 'dlv --help' for usage.
root@03c1977b1063:/go/src/app# dlv debug main.go
could not launch process: fork/exec /go/src/app/__debug_bin: operation not permitted

Итак добавим параметры:

$ docker run -it --rm --security-opt="apparmor=unconfined" --cap-add=SYS_PTRACE my-golang-app bash

И вуаля 🎉
$ root@7dc3a7e8b3fc:/go/src/app# dlv debug main.go
Type 'help' for list of commands.
(dlv)

P.S. опять же этот же трюк можно использовать с docker-compose/ либо с multi-stage билдами. Если интересно как дебажить multi-stage билды на Го просьба поставить “+” в комментариях или кинуть помидором 🍅.
