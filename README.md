# go-snippets

My Go Snippets


# TOC

- [go-snippets](#go-snippets)
- [TOC](#toc)
- [useful links](#useful-links)
- [read values from stdin](#read-values-from-stdin)
- [read line from stdin](#read-line-from-stdin)
- [convert string to int](#convert-string-to-int)
- [convert int to string](#convert-int-to-string)
- [count characters in string](#count-characters-in-string)
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


# read line from stdin

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	var s string
	scanner := bufio.NewScanner(os.Stdin)

	if scanner.Scan() {
		s = scanner.Text()
	}

	fmt.Print(s)
}

```

sample

```bash
% go run tmp.go
hello world
hello world
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


# count characters in string

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	s := "hello world!"
	fmt.Print(strings.Count(s, ""))
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