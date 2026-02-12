### TOC 

[Basic Data Structure](#basic-data-structure)
[Practise](#practise)

#### Basic Data Structure

- [Segment Tree](#segment-tree)

##### Segment Tree

线段树只是一种用于维护区间特定值的数据结构. 一般的线性区间问题可以采用: 前缀和差分, 线性遍历等方式直接维护. 线段树可以在O(logn)时间内进行修改和查找, 对于单次固定线性序列不一定是最有解决方案, 但是对于动态变化的线性序列和动态维护的序列有明显提升.

```go
// list implement
func random_list(n int) []int {
	list := make([]int, n)
	for i := range list {
		list[i] = rand.Intn(100)
	}
	show("origin list", list)
	return list
}

func show(prefix string, list any) {
	fmt.Printf("%s: %+v\n", prefix, list)
}

type (
	SegmentMerge[E cmp.Ordered] func(a, b E) E
	SegmentTree[T cmp.Ordered]  struct {
		data  []T
		tree  []T
		merge SegmentMerge[T]
	}
)

func MaxMerge[E cmp.Ordered](a, b E) E {
	if a > b {
		return a
	}
	return b
}
func MinMerge[E cmp.Ordered](a, b E) E {
	if a < b {
		return a
	}
	return b
}
func SumMerge[E cmp.Ordered](a, b E) E {
	return a + b
}

func NewSegmentTree[T cmp.Ordered](arr []T, merge SegmentMerge[T]) *SegmentTree[T] {
	n := len(arr)
	tn := 2 * n
	if n&(n-1) > 0 {
		// 叶子节点不全在一层: 精确计算为4*n-5, 粗略计算为4*n-1, 可接收情况下可以直接设置为4*n.
		tn = 4 * n
	}
	st := &SegmentTree[T]{
		data:  make([]T, n),
		tree:  make([]T, tn),
		merge: merge,
	}
	_ = copy(st.data, arr)
	st.Build(0, 0, n-1)
	return st
}

func (st SegmentTree[T]) left(cur int) int {
	return cur<<1 + 1
}
func (st SegmentTree[T]) right(cur int) int {
	return cur<<1 + 2
}

func (st *SegmentTree[T]) Build(cur int, l, r int) {
	if l == r {
		st.tree[cur] = st.data[l]
		return
	}
	mid := l + (r-l)/2
	st.Build(st.left(cur), l, mid)
	st.Build(st.right(cur), mid+1, r)
	st.tree[cur] = st.merge(st.tree[st.left(cur)], st.tree[st.right(cur)])
}

func (st *SegmentTree[T]) Find(cur int, ll, lr int, fl, fr int) T {
	if ll == fl && lr == fr {
		return st.tree[cur]
	}
	mid := ll + (lr-ll)/2
	if fr <= mid {
		return st.Find(st.left(cur), ll, mid, fl, fr)
	}
	if fl > mid {
		return st.Find(st.right(cur), mid+1, lr, fl, fr)
	}
	lv, rv := st.Find(st.left(cur), ll, mid, fl, mid), st.Find(st.right(cur), mid+1, lr, mid+1, fr)
	return st.merge(lv, rv)
}

func (st *SegmentTree[T]) Update(i int, v T) {
	if i < 0 || i >= len(st.data) {
		panic("invalid index")
	}
	st.data[i] = v
	st.updateTree(0, 0, len(st.data)-1, i, v)
}

func (st *SegmentTree[T]) updateTree(cur int, l, r int, i int, v T) {
	if l == r {
		st.tree[cur] = v
		return
	}
	mid := l + (r-l)/2
	if i <= mid {
		st.updateTree(st.left(cur), l, mid, i, v)
	} else {
		st.updateTree(st.right(cur), mid+1, r, i, v)
	}
	st.tree[cur] = st.merge(st.tree[st.left(cur)], st.tree[st.right(cur)])
}

func main() {
	/*
	1. 线段树只是维护二分区间, 用于快速解决动态序列问题
	2. 线段树可以是多维度的, 取决于节点的设计, 可以解决不同维度序列问题
	3. 在一般情况下, 泛型线段树可以作为模板, 基于模板进行题意改造即可.
	*/
	n := 10
	arr := random_list(n)
	st := NewSegmentTree(arr, MaxMerge)
	show("segment tree", st)
	fl, fr := 1, n-1
	show("the max value of [0,3]", st.Find(0, 0, n-1, fl, fr))
	st.Update(1, 1<<7)
	show("segment tree", st)
	show("updated", st.Find(0, 0, n-1, fl, fr))
}
```

#### Practise
