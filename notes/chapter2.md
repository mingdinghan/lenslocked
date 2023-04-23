### Dynamic Reloading
- Dynamic reloading means running a tool to watch for changes in the code, then automatically rebuilding and restarting the program once a change is detected.
- when `modd` is run, it will follow the configurations in `modd.conf` to do two things:
  - watch for changes to any `.go` files, including test files, and run tests for any changed directories
  - watch for changes to any non-test `.go` file and to build and restart the app if a change is detected

### HTTP Status Codes
- To set a status code, look at the `http.ResponseWriter` type in the `net/http` package
  - If we call `Write` without setting a status code, the `200 OK` status code will be set by default
  - If we want to set our own status code, we need to call `WriteHeader` before calling `Write`

### The http.Handler Type
- When `http.HandleFunc` is called to register a handler, `DefaultServeMux` is used behind the scenes
```go
// from 'net/http'
func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
  DefaultServeMux.HandleFunc(pattern, handler)
}
```

- `http.Handler` is an interface type with a `ServeHTTP` method
```go
// from 'net/http'
type Handler interface {
  ServeHTTP(ResponseWriter, *Request)
}
```

- The second argument to `ListenAndServe` can be a `http.Handler` interface
  - If `nil` is passed, `DefaultServeMux` is used
  - we cannot just pass in any function that has arguments `(ResponseWriter, *Request)` into `ListenAndServe` - need to create a type that implements the `http.Handler` interface, i.e. implements the `ServeHTTP` method
```go
func ListenAndServe(addr string, handler Handler) error
```

### The http.HandlerFunc Type
- `net/http` also provides the `http.HandlerFunc` function type, which also implements the `Handler` interface
```go
type HandlerFunc func(ResponseWriter, *Request)

func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request) {
  f(w, r)
}
```
- any handler function, such as `pathHandler(w http.ResponseWriter, r *http.Request)`, can be converted into `HandlerFunc`
  - which has a `ServeHTTP` method which implements the `Handler` interface
```go
http.ListenAndServe(":3000", http.HandlerFunc(pathHandler))
```
