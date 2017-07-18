## 给httprouter添加pprof

1：获取包

```sh
go get github.com/feixiao/httpprof
```

2：进入所在目录，获取依赖

```
 govendor sync
```

3：编译运行example

```
go build
```

4：外部项目添加使用，只需要参考example的使用即可

```go
func Index(w http.ResponseWriter, r *http.Request, _ httprouter.Params) {
	fmt.Fprint(w, "Welcome!\n")
}

func Hello(w http.ResponseWriter, r *http.Request, ps httprouter.Params) {
	fmt.Fprintf(w, "hello, %s!\n", ps.ByName("name"))
}

func main() {
	router := httprouter.New()

	// 在原来的httprouter的使用基础只是添加了这一句代码
	router = httpprof.WrapRouter(router)

	router.GET("/", Index)
	router.GET("/hello/:name", Hello)

	log.Fatal(http.ListenAndServe(":8080", router))
}
```