# Concurrency

## defer

- used as prefix. Will lead that the function after `defer` will be called at the end of the function it is wrapped into.

### Example

```go
func main(){
    server := httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.WriteHeader(http.StatusOK)
    }))
    defer server.Close()
    fmt.Println("Do sth.")
    fmt.Println("Do sth. else")
    // server will be closed now
}
```

## go routines and channels

- `go func(parameter type...){}` creates a new go routine, means runs in parallel
- `go func(parameter type...){}()` '()' immediately calls the anonymous function
- `make(chan type)` creates a channel for the given type. A channel is used to send the go routines to, where the routines will be executed
- `<-` sends routines to a channel or pulls out results from a channel

### Example

```go
package concurrency

type WebsiteChecker func(string) bool
type result struct {
    string
    bool
}

func CheckWebsites(wc WebsiteChecker, urls []string) map[string]bool {
    results := make(map[string]bool)
    resultChannel := make(chan result)

// it is not possible to use url in the go func(). When using url in go func() only the last reference of urls would be used
// to avoid this: declare a parameter in the func, so that each url will be copied (new reference will be created)
    for _, url := range urls {
        go func(u string) { // declare go routine with anonymous function
            resultChannel <- result{u, wc(u)} // write a new result which is composed by the result of 'u' and 'wc(u)'
        }(url) // immediately call the functio
    }

    for i := 0; i < len(urls); i++ { // iterate over the *amount* of structs which are stored in the channel
        result := <-resultChannel // pull out struct from channel; this is a blocking call
        results[result.string] = result.bool
    }

    return results
}
```

## select or synchronising processes

- `select{}` waits on multiple created channels and executes the code once the first channel is finished (which looks similiar to a switch)
- timeouts can be set with their own case: `case <- time.After():`

## Example

```go
func Racer(a, b string) (winner string) {
    select {
    case <-ping(a):
        return a
    case <-ping(b):
        return b
    case <-time.After(10 * time.Second): //sets a timeout; opens a new channel which will be terminated after n seconds
        return "", fmt.Errorf("timed out waiting for %s and %s", a, b)
    }
}

func ping(url string) chan struct{} {
    ch := make(chan struct{}) // create a channel
    go func() {
        http.Get(url)
        close(ch) // close the channel as soon as http.Get() is done
    }()
    return ch
}
```
