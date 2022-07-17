# Two complement's exception

Reading a golang issue related to how two complement works https://github.com/golang/go/issues/51414 
Most of the time, we don't need to care about how two complement works, but this issue proves the opposite, 
we do need to care because of two complement algorithm has **an exception** : The most negative integer could not be negate.
The reason could be obviously explained using a 8 bit integer:

- −128:	1000 0000
- invert bits:	0111 1111
- add one:	1000 0000

Another explanation could be: negate(min 8 bit) integer is different by 1 comparing to (max 8 bit) integer: 127 vs -128.


```golang
import "fmt"

func main() {
	var n int64
	n = -(1 << 63)
	fmt.Println(n)
	fmt.Println(-n)
}

-9223372036854775808
-9223372036854775808
```

