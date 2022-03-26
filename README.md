# go-snippets

My Go Snippets


# TOC

- [go-snippets](#go-snippets)
- [TOC](#toc)
- [useful links](#useful-links)
- [hello world](#hello-world)
- [read values from stdin](#read-values-from-stdin)
- [read line from stdin](#read-line-from-stdin)
- [convert string to int](#convert-string-to-int)
- [convert int to string](#convert-int-to-string)
- [count characters in string](#count-characters-in-string)
- [print current time](#print-current-time)
- [max find from array or slice](#max-find-from-array-or-slice)
- [generate random number](#generate-random-number)
- [reversing slice](#reversing-slice)
- [trim line feed](#trim-line-feed)
- [goroutine in for loop](#goroutine-in-for-loop)
- [generate uuidv4](#generate-uuidv4)
- [call sts get-caller-identity with debug log using aws-sdk-go](#call-sts-get-caller-identity-with-debug-log-using-aws-sdk-go)


# useful links

- https://play.golang.org/
- https://pkg.go.dev/
- https://github.com/golang/go
- https://gobyexample.com/


# hello world

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, World")
}

```


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


# print current time

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	fmt.Println(time.Now())
}

```


# max find from array or slice

```go
func findMax(n ...int) int {
	max := 0
	for _, value := range n {
		if value > max {
			max = value
		}
	}
	return max
}

func main() {
	findMax(1, 3, 5)
}
```


# generate random number

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func main() {
	rand.Seed(time.Now().UnixNano())
	fmt.Println(rand.Intn(10))
}

```


# reversing slice

```go
package main

import "fmt"

func main() {
	a := []int{1, 2, 3, 4, 5, 6}
	fmt.Println(a) // [1 2 3 4 5 6]
	for i := len(a)/2 - 1; i >= 0; i-- {
		opp := len(a) - 1 - i
		a[i], a[opp] = a[opp], a[i]
	}
	fmt.Println(a) // [6 5 4 3 2 1]
}
```

SliceTricks · golang/go Wiki  
https://github.com/golang/go/wiki/SliceTricks#reversing


# trim line feed

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	rawText := "Hello\nWorld!"
	text := strings.ReplaceAll(rawText, "\n", " ")
	fmt.Println(text)
}
```

strings package - strings - pkg.go.dev  
https://pkg.go.dev/strings#ReplaceAll


# goroutine in for loop

bind local value

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	values := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

	for _, value := range values {
		i := value
		go func() {
			fmt.Println(i)
		}()
	}

	time.Sleep(1 * time.Second)
}
```

---

args in anonymous function

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	values := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

	for _, value := range values {
		go func(i int) {
			fmt.Println(i)
		}(value)
	}

	time.Sleep(1 * time.Second)
}
```

---

isolate function

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	values := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

	myFunc := func(i int) {
		fmt.Println(i)
	}

	for _, value := range values {
		go myFunc(value)
	}

	time.Sleep(1 * time.Second)
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


# call sts get-caller-identity with debug log using aws-sdk-go

```go
package main

import (
	"github.com/aws/aws-sdk-go/aws"
	"github.com/aws/aws-sdk-go/aws/session"
	"github.com/aws/aws-sdk-go/service/sts"

	"fmt"
)

func main() {
	sess, err := session.NewSession(&aws.Config{
		Region:   aws.String("ap-northeast-1"),
		LogLevel: aws.LogLevel(aws.LogDebug),
	})

	svc := sts.New(sess)
	input := &sts.GetCallerIdentityInput{}

	result, err := svc.GetCallerIdentity(input)
	if err != nil {
		fmt.Println(err.Error())
		return
	}

	fmt.Println(result)
}

```

