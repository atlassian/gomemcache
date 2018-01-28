gomemcache
===========
[![Build Status](https://travis-ci.org/zeayes/gomemcache.svg?branch=master)](https://travis-ci.org/zeayes/gomemcache)

A memcached client supported binary and text protocol implements by golang.

Install
===========
```bash
go get -u github.com/zeayes/gomemcache
```

Note
===========
This libary currently supports *set* *get* *cas* *add* *replace* *delete*, can be used one memcached instance.

Demo
===========
```go
package main

import (
	"fmt"
	"log"

	"github.com/zeayes/gomemcache"
)

func main() {
	client, err := gomemcache.NewClient([]string{"127.0.0.1:11211"})
	if err != nil {
		log.Fatalf("init client error: %v", err)
	}
	// set client protocol "text" or "binary", default is "text"
	// client.SetProtocol("binary")
	item := &gomemcache.Item{Key: "test1", Flags: 9, Expiration: 5, Value: []byte("replace_value")}
	if err = client.Set(item); err != nil {
		log.Fatalf("Set error: %v", err)
	}
	it, err := client.Get("test1")
	if err != nil {
		log.Fatalf("Get error: %v", err)
	}
	fmt.Println(it)
	items, err := client.MultiGet([]string{"test1", "test2", "test3"})
	if err != nil {
		log.Fatalf("MultiGet error: %v", err)
	}
	fmt.Println(items)
}
```

Benchmark
===========
benchmark on MBP(Mid 2015 2.2 GHz 16GB), and memcached served by default options.
```
BenchmarkBinarySet-8        	   50000	     37790 ns/op	     120 B/op	       4 allocs/op
BenchmarkBinaryGet-8        	   30000	     45864 ns/op	     480 B/op	      15 allocs/op
BenchmarkBinaryMultiGet-8   	   20000	     89126 ns/op	    4416 B/op	      75 allocs/op
BenchmarkTextSet-8          	  200000	      6937 ns/op	     120 B/op	       4 allocs/op
BenchmarkTextGet-8          	   30000	     42921 ns/op	    4488 B/op	      15 allocs/op
BenchmarkTextMultiGet-8     	   30000	     54572 ns/op	    6144 B/op	      54 allocs/op
```

License
===========

gomemcache is released under the MIT License.
