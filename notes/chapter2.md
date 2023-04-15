### Dynamic Reloading
- Dynamic reloading means running a tool to watch for changes in the code, then automatically rebuilding and restarting the program once a change is detected.
- when `modd` is run, it will follow the configurations in `modd.conf` to do two things:
  - watch for changes to any `.go` files, including test files, and run tests for any changed directories
  - watch for changes to any non-test `.go` file and to build and restart the app if a change is detected

### HTTP Status Codes
- To set a status code, look at the `http.ResponseWriter` type in the `net/http` package
  - If we call `Write` without setting a status code, the `200 OK` status code will be set by default
  - If we want to set our own status code, we need to call `WriteHeader` before calling `Write`
