# go-snippets

My Go Snippets


# TOC

- [go-snippets](#go-snippets)
- [TOC](#toc)
- [useful links](#useful-links)
- [read values from stdin](#read-values-from-stdin)
- [convert string to int](#convert-string-to-int)
- [convert int to string](#convert-int-to-string)
- [generate uuidv4](#generate-uuidv4)


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


# convert string to int

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	s := "777"
	i, _ := strconv.Atoi(s)
	fmt.Printf("%T", i)
}

```


# convert int to string

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	i := 777
	s := strconv.Itoa(i)
	fmt.Printf("%T", s)
}

```


# generate uuidv4

```go
package main

import (
	"fmt"

	"github.com/google/uuid"
)

func main() {
	uuidv4, _ := uuid.NewRandom()
	fmt.Print(uuidv4)
}
```