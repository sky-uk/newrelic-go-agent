## NewRelic Go Agent

A small convenience layer that sits on top of [newrelic-go-agent](https://github.com/paulsmith/newrelic-go-agent), to make
it easy to create transactions for NewRelic in Go.

## Example Usage

``` golang
import "github.com/remind101/nra"

func main() {
    nra.Init("My App", "<new relic license key>")

    // Add to a context.Context
    // https://godoc.org/golang.org/x/net/context
    tx := nra.NewTx("/my/transaction/name")
    tx.Start()
    defer tx.End()

    ctx := context.Background()
    ctx = nra.WithTx(ctx, tx)

}

// Add a segment to the current transaction if one exists
func FindAllUsers(ctx context.Context, ) ([]User, error) {
    tx, ok := nra.FromContext(ctx)
    if ok {
        // Start a datastore segment
        tx.StartDatastore("users", "SELECT", "SELECT * from users WHERE id = 1", "FindAllUsers")
        defer tx.EndSegment()
    }

    // look up users, etc
}

// Add as middleware to your http server
func WithNRA(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        tx := nra.NewTx("/my/transaction/name (GET)")
        tx.Start()
        defer tx.End()
        next.ServeHTTP(w, r)
    })
}



```



## Software Credits

The development of this software was made possible using the following components:

[newrelic-go-agent](https://github.com/paulsmith/newrelic-go-agent) by Paul Smith 
Licensed Under: Apache License, Version 2.0