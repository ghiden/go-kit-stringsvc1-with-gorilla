# Go kit: stringsvc1 with http method check and gorilla

[Go kit](https://gokit.io)  
[stringsvc](https://gokit.io/examples/stringsvc.html)  
[Gorilla Web Toolkit](http://www.gorillatoolkit.org)  

This is an iteration of [method check example](https://github.com/ghiden/go-kit-stringsvc1) using Gorilla Web Toolkit.

```
go build -o server
./server
```

```
$ curl -v localhost:8080/uppercase
*   Trying ::1...
* Connected to localhost (::1) port 8080 (#0)
> GET /uppercase HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.43.0
> Accept: */*
>
< HTTP/1.1 405 Method Not Allowed
< Allow: POST
< Content-Type: text/plain; charset=utf-8
< X-Content-Type-Options: nosniff
< Date: Thu, 11 May 2017 04:08:43 GMT
< Content-Length: 19
<
Method not allowed
```

```go
func methodHandlerWrapper(s *httptransport.Server) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		s.ServeHTTP(w, r)
	})
}
```

```go
r.Path("/uppercase").Handler(handlers.MethodHandler{"POST": methodHandlerWrapper(uppercaseHandler)})
```
