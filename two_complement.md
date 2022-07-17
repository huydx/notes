# Two complement's exception

Reading a golang issue related to how two complement works https://github.com/golang/go/issues/51414 
Most of the time, we don't need to care about how two complement works, but this issue proves the opposite, 
we do need to care because of two complement algorithm has **an exception** : The most negative integer could not be negate.
The reason could be obviously explained using a 8 bit integer (overflow):

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

In wiki, there're some extra explanations that are mathematically interesting:

>Mathematically, this is complementary to the fact that the negative of 0 is again 0. For a given number of bits k there is an even number of binary numbers 2k, taking negatives is a group action (of the group of order 2) on binary numbers, and since the orbit of zero has order 1, at least one other number must have an orbit of order 1 for the orders of the orbits to add up to the order of the set. Thus some other number must be invariant under taking negatives (formally, by the orbit-stabilizer theorem). Geometrically, one can view the k-bit binary numbers as the cyclic group {\displaystyle \mathbb {Z} /2^{k}}{\displaystyle \mathbb {Z} /2^{k}}, which can be visualized as a circle (or properly a regular 2k-gon), and taking negatives is a reflection, which fixes the elements of order dividing 2: 0 and the opposite point, or visually the zenith and nadir.
