# go-snippets

My Go Snippets


# TOC

- [go-snippets](#go-snippets)
- [TOC](#toc)
- [useful links](#useful-links)
- [read values from stdin](#read-values-from-stdin)


# useful links

- https://play.golang.org/
- https://pkg.go.dev/
- https://github.com/golang/go


# read values from stdin

```go
package main

import (
	"fmt"
)

func main() {
	var s1, s2 string
	fmt.Scan(&s1, &s2)
	fmt.Println(s1, s2)
}

```

sample

```bash
% go run tmp.go
hoge fuga
hoge fuga
% go run tmp.go
hoge
fuga
hoge fuga
```
