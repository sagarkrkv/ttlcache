## TTLCache - an in-memory cache with expiration

TTLCache is a simple key/value cache in golang with the following functions:

1. Thread-safe
2. Individual expiring time or global expiring time, you can choose
3. Auto-Extending expiration on `Get`
4. Fast and memory efficient
5. Can trigger callback on key expiration

[![Build Status](https://travis-ci.org/diegobernardes/ttlcache.svg?branch=master)](https://travis-ci.org/diegobernardes/ttlcache)

#### Usage
```go
import (
  "time"
  "fmt"

  "github.com/diegobernardes/ttlcache"
)

func main () {
  expirationCallback := func(key string, value interface{}) {
		fmt.Printf("This key(%s) has expired\n", key)
	}

  cache := ttlcache.NewCache()
  cache.SetTTL(time.Duration(10 * time.Second))
  cache.SetExpirationCallback(expirationCallback)

  cache.Set("key", "value")
  cache.SetWithTTL("keyWithTTL", "value", 10 * time.Second)

  value, exists := cache.Get("key")
  count := cache.Count()
  result := cache.Remove("key")
}
```

## Requirement

Tested in ´go1.4.2´ and the project is with 100% test coverage

## TODO

- Comment the code
- Add a roadmap
- Add benchmarks
- Improve map performance

## License

TTLCache is released under the [MIT License](http://www.opensource.org/licenses/MIT).

#### Original Project

TTLCache was forked from [wunderlist/ttlcache](https://github.com/wunderlist/ttlcache) to add extra functions not avaiable in the original scope.
The main differences are:

1. A item can store any kind of object, previously, only strings could be saved
2. There is a option to add a callback to get key expiration
3. The expiration can be either global or per item
4. Can exist items without expiration time
5. Expirations and callbacks are realtime. Don't have a pooling time to check anymore, now it's done with a heap.
