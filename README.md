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
- [json encoding, decoding](#json-encoding-decoding)
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


# json encoding, decoding

Marshal encoding => struct to json

```go
package main

import (
	"encoding/json"
	"fmt"
)

type ColorGroup struct {
	ID     int
	Name   string
	Colors []string
}

func main() {
	myGroup := ColorGroup{
		ID:   1,
		Name: "Reds",
		Colors: []string{
			"Crimson",
			"Red",
			"Ruby",
			"Maroon",
		},
	}

	bytes, err := json.Marshal(myGroup)
	if err != nil {
		fmt.Println(err)
	}

	fmt.Println(string(bytes))
}
```

---

Unmarshal decoding => json to struct

```go
package main

import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"net/http"
)

type Response struct {
	Page       int `json:"page"`
	PerPage    int `json:"per_page"`
	Total      int `json:"total"`
	TotalPages int `json:"total_pages"`
	Data       []struct {
		ID        int    `json:"id"`
		Email     string `json:"email"`
		FirstName string `json:"first_name"`
		LastName  string `json:"last_name"`
		Avatar    string `json:"avatar"`
	} `json:"data"`
	Support struct {
		URL  string `json:"url"`
		Text string `json:"text"`
	} `json:"support"`
}

func PrettyPrint(i interface{}) string {
	s, _ := json.MarshalIndent(i, "", "\t")
	return string(s)
}

func main() {

	resp, err := http.Get("https://reqres.in/api/users?page=2")
	if err != nil {
		fmt.Println(err)
	}
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body) // response body is []byte

	var result Response
	if err := json.Unmarshal(body, &result); err != nil { // Parse []byte to the go struct pointer
		fmt.Println(err)
	}

	fmt.Println(PrettyPrint(result))
	// fmt.Println(result)

	// loop
	// for _, data := range result.Data {
	// 	fmt.Println(data.FirstName)
	// }
}
```

Reqres - A hosted REST-API ready to respond to your AJAX requests  
https://reqres.in/

json package - encoding/json - pkg.go.dev  
https://pkg.go.dev/encoding/json

JSON and Go - The Go Programming Language  
https://go.dev/blog/json

JSON-to-Go: Convert JSON to Go instantly  
https://mholt.github.io/json-to-go/


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

