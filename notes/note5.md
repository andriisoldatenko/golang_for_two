Note #5 Дебажим приложения в multi-staged докере 🐳

Давайте представим что на проде у вас есть микросервис, каждый из которых живет в своем Dockerfile и естественно, как у всех взрослых дядь - это multi-stage Dockerfile. Более подробно о multi-stage можно прочитать в доках ( https://docs.docker.com/develop/develop-images/multistage-build/), если лень - то этопросто Dockerfile у которого есть 2 FROM ключевых слова и в мы что-то копируем из одного в другой слой.

Итак приступим 🐎:
```
docker build -t goapp:latest .
Sending build context to Docker daemon  22.53kB
Step 1/7 : FROM golang AS builder
...
Successfully tagged goapp:latest
```

Каждый день нам нужно разрабатывать и дебажить это приложение: внимательный читатель может предложить установить дебаггер внутри одного из слоев. Итак добавим что-то вроде:


```
--- a/Dockerfile
+++ b/Dockerfile
@@ -1,4 +1,5 @@
 FROM golang AS builder
+RUN go get -u github.com/go-delve/delve/cmd/dlv
 ADD . /src
 RUN cd /src && go build -o goapp
```

И затем запустим что-то вроде этого:

```
➜  debug-multi-stage-docker-and-go docker run -it --entrypoint=dlv goapp
docker: Error response from daemon: OCI runtime create failed: container_linux.go:344: starting container process caused "exec: \"dlv\": executable file not found in $PATH": unknown.
(mlflow) ➜  debug-multi-stage-docker-and-go pyenv deactivate
```

И вот незадача, та же история будет если у вас `docker-compose` для локальной разработки или что-то еще. Как быть)
Есть замечательный флаг --target у docker build!

```
docker build --target builder -t goapp:latest .
docker run -it goapp sh
# dlv debug main.go
could not launch process: fork/exec /src/__debug_bin: operation not permitted
```
А как пофиксить это ☝, читай мой прошлый пост https://t.me/golang_for_two/20 )

И Вуаля! Огромный Плюс ⚡️ данного подхода лично для меня, отсутствие Dockerfile-dev и вариантов.


P.S. Если же у тебя docker-compose, то в своем docker-compose.override.yml можно написать так:
```
version: "3.4"
services:
  app:
    image: goapp:dev
    build:
      context: .
      dockerfile: Dockerfile
      target: builder 
```
🎉💥🎉
