### TOC

[Conceptual Implementation](#conceptual-implementation)
[Question Solution](#question-solution)

#### Conceptual Implementation

- [Sort](#sort)
- [Trace Back](#trace-back)
- [Dynamic Programming](#dynamic-programming)
- [Prefix Sum](#prefix-sum)

##### Sort

聚焦`insert_sort, heap_sort, quick_sort, merge_sort`的一般实现. (按照最佳概念实现, 基本适用于多种情况, 且可以相互组合)这里使用`golang`实现默认升序排序.

```go
// insert sort
func insert_sort(list []int) {
    n := len(list)
    for i:=1;i<n;i++ {
        x := list[i]
        var j int
        for j=i;j>0;j-=1 {
            if list[j]<list[j-1] {
                list[j] = list[j-1]
            }else{
                break
            }
        }
        list[j] = x
    }
}
// heap sort
func down_update(list []int, ll int, i int) {
    for {
        max_p := i
        l,r := i<<1+1, i<<1+2
        if l<ll && list[l]>list[max_p] {max_p = l}
        if r<ll && list[r]>list[max_p] {max_p = r}
        if max_p == i  {break}
        list[i],list[max_p] = list[max_p],list[i]
        i = max_p
    }
}

func heap_sort(list []int) {
    n := len(list)
    // setup heap
    for i:=n/2-1;i>=0;i-- {
        down_update(list, n, i)
    }
    // heap sort
    for i:=n-1;i>0;i-- {
        list[i],list[0] = list[0],list[i]
        down_update(list, i, 0)
    }
    // top k
    ans := make([]int, 0, k)
    for k>0 {
        ans = append(ans, list[0])
        list[0] = list[n-1]
        n--
        down_update(list, n, 0)
        k--
    }
}
// quick sort
func find_base(list []int, l,r int) int {
    x := list[l]
    for l<r {
        for r > l && list[r] > x {r--}
        list[l] = list[r]
        l++
        for l < r && list[l] < x {l++}
        list[r] = list[l]
        r--
    }
    list[l] = x
    return l
}
func quick_sort(list []int, l,r int) {
    if l>=r {
        return
    }
    p := find_base(list,l,r)
    quick_sort(list, l, p-1)
    quick_sort(list, p+1, r)
}
// call quick sort with quick_sort(list, 0, len(list)-1)

// merge sort
func merge(l1, l2 []int) []int {
    m,n := len(l1),len(l2)
    ret := []int{}
    i,j := 0,0
    for i<m && j<n {
        if l1[i]<=l2[j] {
            ret = append(ret, l1[i])
            i++
        }else{
            ret = append(ret, l2[j])
            j++
        }
    }
    for i<m {
        ret = append(ret, l1[i])
        i++
    }
    for j<n {
        ret = append(ret, l2[j])
        j++
    }
    return ret
}

func merge_sort(list []int) []int {
    n := len(list)
    if n<2 {
        return list
    }
    mid := n/2
    ll := merge_sort(list[:mid])
    lr := merge_sort(list[mid:])
    return merge_sort(ll,lr)
}
```

##### Trace Back

递归-回溯-剪枝的核心在于: 如何递推, 如何返回, 如何剪枝. 回溯类算法一般用于穷举所有的可能, 这一般针对可能性问题, 或者校验性问题. 通常搭配dp进行快速记忆化剪枝.

```go
// golang中的一般模板
func <>(ss sequence) [sequence|value] {
    ans := [sequence|value]
    var dfs func(ctx context) 
    dfs = func(ctx context) {
        ...
    }
    dfs(ctx)
    return ans
}
// eg-1: 全排列
func Permutation(n int) [][]int {
    ans := make([][]int,0)
    se := []int{}
    vi := make([]bool,n+1)
    var dfs func() 
    dfs = func() {
        if len(se) == n {
            ans = append(ans, append([]int(nil),se...))
        }
        for i:=1;i<=n;i++ {
            if !vi[i] {
                vi[i] = true
                se = append(se, i)
                dfs()
                se = se[:len(se)-1]
                vi[i] = false
            }
        }
    }
    dfs()
    return ans
}
// eg-2: 所有子集
func AllSubSets(nums []int) [][]int {
    n := len(nums)
    ans := make([][]int,0)
    se := []int{}
    var dfs func(index int)
    dfs = func(index int) {
        if index<=n {
            ans = append(ans, append([]int(nil),se...))
        }
        for i:=index;i<n;i++ {
            se = append(se, nums[i])
            dfs(i+1)
            se = se[:len(se)-1]
        }
    }
    dfs(0)
    return ans
}
// eg-3: 有效括号
func (n int) []string {
    ans := make([]string,0)
    se := ""
    var dfs func(l,r int)
    dfs = func(l,r int) {
        if l==0 && r==0 {
            t := se
            ans = append(ans, t)
        }
        if r<l {return}
        if l>0 {
            se += "("
            dfs(l-1,r)
            se = string(se[:len(se)-1])
        }
        if r>0 {
            se += ")"
            dfs(l,r-1)
            se = string(se[:len(se)-1])
        }
    }
    dfs(n,n)
    return ans
}
// eg-4: n皇后
func Queen(n int) [][]string {
    ans := make([][]string, 0)
    se := []string{}
    temp := ""
    for i:=0;i<n;i++ {
        temp += "."
    }
    var canPlace func(x,y int) bool
    var dfs func(index int) 
    canPlace = func(x,y int) bool {
        for i:=0;i<y;i++ {
            if se[i][x] == 'Q' {
                return false
            }
        }
        l,r := x-1, x+1
        for i:=y-1;i>=0;i-- {
            if (l>=0 && se[i][l] == 'Q') || (r<n && se[i][r] == 'Q') {return false}
            l--
            r++
        }
        return true
    }
    dfs = func(index int) {
        if len(se) == n {
            ans = append(ans, append([]string(nil),se...))
        }
        se = append(se, temp)
        for i:=0;i<n;i++ {
            if canPlace(i,index) {
                se[index][i] = string(temp[:i]) + "Q" + string(temp[i+1:])
                dfs(index+1)
            }
        }
        se = se[:len(se)-1]
    }
    dfs(0)
    return ans
}
```

##### Dynamic Programming

动态规划(表格化处理)一般处理具备强线性相关的问题, 这集中在存在一定的线性递推式中, 依赖状态转移式的合理性. 注意: 这里的表格化具备强烈的语义而不是单纯的位置关系. 

##### Prefix Sum

前缀和一般结合差分用于得到连续区间的总值. 主要使用的是线性前缀和哈希前缀.

```go
// list prefix sum
func list_prefix(nums []int) []int {
    prefix_sum := make([]int,0,len(nums))
    sum := 0
    for i:=0;i<len(nums);i++ {
        prefix_sum[i] = sum
        sum += nums[i]
    }
    return prefix_sum
}
// hash prefix sum
func hash_prefix(nums []int) map[int]int {
    hash_sum := make(map[int]int) // hash一般情况: [index,prefix_sum], [prefix_sum,count], [prefix_sum,list]...
    sum := 0
    for i:==range nums {
        hash_sum[i] = sum
        sum += nums[i]
    }
    return hash_sum
}
// usage
func main() {
    // list[i,j]
    list := list_prefix(...)
    sum = list[j]-list[i]+nums[j]
    // hash[i,j]
    // hash依赖不同的[k,v]可以兼容更多实现.
    hash := hash_prefix(...)
    sum = hash[j]-hash[i]+nums[j]
}
```

#### Question Solution
