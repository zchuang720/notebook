# iota

### 总结

1. 不同 const 定义块互不干扰；
2. 所有注释行和空行全部忽略；
3. 没有表达式的常量定义复用上一行的表达式；
4. 从第一行开始，iota 从 0 逐行加一；
5. 替换所有 iota。

### 示例
```
const a = iota

const (
	b = iota
)

const (
	c = 10
	d = iota
	e
	f = "hello"
	// nothing
	g
	h = iota
	i
	j = 0
	k
	l, m = iota, iota
	n, o

	p = iota + 1
	q
	_
	r = iota * iota
	s
	t = r
	u
	v = 1 << iota
	w
	x = iota * 0.01
	y float32 = iota * 0.01
	z
)
```
分析
```
const (
	c = 10 // iota = 0
	d = 1 // iota = 1
	e = 2 // iota = 2
	f = "hello" // iota = 3
	g = "hello" // iota = 4
	h = 5 // iota = 5
	i = 6 // iota = 6
	j = 0 // iota = 7
	k = 0 // iota = 8
	l, m = 9, 9 // iota = 9
	n, o = 10, 10 // iota = 10
	p = 11 + 1 // iota = 11
	q = 12 + 1 // iota = 12
	_ = 13 + 1 // iota = 13
	r = 14 * 14 // iota = 14
	s = 15 * 15 // iota = 15
	t = r // iota = 16
	u = r // iota = 17
	v = 1 << 18 // iota = 18
	w = 1 << 19 // iota = 19
	x = 20 * 0.01 // iota = 20
	y float32 = 21 * 0.01 // iota = 21
	z float32 = 22 * 0.01 // iota = 22
)
```