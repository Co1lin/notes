# DSA

_Note that in this document there are some images with transparent backgound, and a bright theme may be more suitable to read them._

_Click on a tile to change the color scheme_:

<div class="tx-switch">
  <button data-md-color-scheme="default"><code>default</code></button>
  <button data-md-color-scheme="slate"><code>slate</code></button>
</div>

<script>
  var buttons = document.querySelectorAll("button[data-md-color-scheme]")
  buttons.forEach(function(button) {
    button.addEventListener("click", function() {
      var attr = this.getAttribute("data-md-color-scheme")
      document.body.setAttribute("data-md-color-scheme", attr)
      var name = document.querySelector("#__code_0 code span:nth-child(7)")
      name.textContent = attr
    })
  })
</script>

## Basis of Performance Analysis

### Notation

$T(n)$：为求解规模为n的问题，所需执行的基本操作的次数。

$O(f(n))$：$T(n) = O(f(n)) ~ \Leftrightarrow T(n) < c \cdot f(n)$，i.e. $f(n)$是上界。

$\Omega(f(n))$：$T(n) = \Omega(f(n)) ~ \Leftrightarrow T(n) > c \cdot f(n)$，i.e. $f(n)$是下界。

$\Theta(f(n))$：$T(n) = \Theta(f(n)) ~ \Leftrightarrow c_1 \cdot f(n)> T(n) > c_2 \cdot f(n)$，i.e. $f(n)$与$T(n)$同阶。

![notation](DSA_Review.assets/notation.png)

### NP = P ?

P问题：可以在多项式时间内求解。

NP问题：能在多项式时间内验证得出正确解的问题。

### Series and Corresponding Complexity

#### 常用级数：算术、幂、几何、对数等

![Screen Shot 2020-12-27 at 9.16.16 PM](DSA_Review.assets/Screen%20Shot%202020-12-27%20at%209.16.16%20PM.png)

![Screen Shot 2020-12-27 at 9.18.13 PM](DSA_Review.assets/Screen%20Shot%202020-12-27%20at%209.18.13%20PM.png)

#### 不常见的分数级数

![Screen Shot 2020-12-27 at 9.16.48 PM](DSA_Review.assets/Screen%20Shot%202020-12-27%20at%209.16.48%20PM.png)

### Master Thm.

不同分治对应的复杂度：

分治通常递推形式：$T(n) = a \cdot T(\frac{n}{b}) + O(f(n))$

即：原问题被分为a个规模均为$n/b$的子任务；（每一层）任务的划分、解的合并耗时$f(n)$。

1. $f(n) = O(n^{log_b a - \epsilon}) ~ \Rightarrow T(n) = \Theta(n^{log_b a})$ （$f(n)$要小于O中的表达式，因为O是其上界）

   e.g. kd-search: $T(n) = 2T(n/4)+O(1) = O(\sqrt{n})$

2. $f(n) = O(n^{log_b a} \cdot log^k n) ~ \Rightarrow T(n) = \Theta(n^{log_b a} \cdot log^{k+1}n)$ 

3. $f(n) = \Omega(n^{log_b a + \epsilon}) ~ \Rightarrow T(n) = \Theta(f(n))$ 

## Binary Search

Optimization:

1. Only one comparison in each loop.
2. Return the maximum element that is not greater than e.

```c++
BinarySearch(data, e, lo, hi):	// [lo, hi)
{
	while (lo < hi)
	{
		median = (lo + hi) / 2
		if (e < data[median])
			hi = mi	// [lo, mi)
		else // (data[median] <= e)
			lo = mi + 1	// [mi + 1, hi) = (mi, hi)
	}	// break when (lo == hi)
	return (lo - 1)	
}
```

Correctness:

以下陈述在算法执行全过程中总是对的：

1. `A[lo - 1]`是已知的小于等于e的数（`A[0] ~ A[lo - 1]`）中最大的
2. `A[hi]`是已知的大于e的数（`A[hi] ~ A[n]`）中最小的

据此，可以以初始状态成立为基，进行数学归纳；按照循环中的两个分支分别归纳，发现无论哪个分支，两个条件仍然是对的。因此最后`A[lo - 1]`是已知的小于等于e的数中最大的。

![Screen Shot 2020-12-27 at 10.11.49 PM](DSA_Review.assets/Screen%20Shot%202020-12-27%20at%2010.11.49%20PM.png)

## Sort Algorithms

### BubbleSort

$O(n^2)$。严格大小关系就是稳定的。

![image-20210105100906401](DSA_Review.assets/image-20210105100906401.png)

#### Improvement

若某一趟扫描未发现逆序对，则说明已经都有序排列了，可以提前终止。

进一步改进：虽然红色unsorted部分未有序，但是它的某个后缀可能有序。维护一个last值，初始值为lo，每次发现逆序对i，i+1时，就令`last=i`。如此从前向后扫描一趟之后，last记录的就是最后一个逆序对的位置。如此，逆序对只可能存在于`[lo, last)`中。特殊地，如果整趟扫描没有发现逆序对，则last仍然为lo值。之后的`hi=last`操作会使`hi == lo`，从而终止循环。

![Screen Shot 2021-01-05 at 10.17.48 AM](DSA_Review.assets/Screen%20Shot%202021-01-05%20at%2010.17.48%20AM.png)

### MergeSort

$O(nlogn)$。优先左边的元素进入归并，稳定。





### QuickSort

$O(nlogn)$。不稳定！



### RadixSort

低位优先的基数排序。

数据规模为n，数字位数为t，每一位的范围为$(0, M]$。

复杂度$O(t*(n+M))$。**稳定！**

局限性：只能比较数字。

### SelectionSort



### ShellSort





## Tree

深度$depth(v)=|path(v)|$，到根节点的路径的**边的数目**。$depth(root)=0$。

高度$height(T)$：树T的所有节点深度的最大值。只有根的树：0；空树：-1。

度$degree$：children的个数。

## AVL Tree

### Definition

平衡因子=左子树高度-右子树高度，绝对值在1内

### Algorithm

#### Insert

从插入节点（叶子）的parent（hot）开始网上走，维护高度，直到第一个失衡节点g；通过`tallerChild(tallerChild(g))`确定p，v，然后用一次3+4重构重平衡。

由于**“高度复原”**性质，整个insert最多1次3+4重构（2次rotate）。

$O(logn)$。

#### Delete

用BST的删除，删除x，记录hot。

从hot向上，一旦发现g失衡，通过`tallerChild(tallerChild(g))`确定p，v，然后用一次3+4重构重平衡。

失衡可能传播，需要一直向上，执行多次3+4重构。

$O(logn)$。





## B Tree

### Definition

m阶B-Tree，每个节点有n<=m-1个key，n+1<=m个分支。（m>=3）

需满足$m\ge n+1 \ge ceil(m/2)$。（**除了根节点**不能少于一半）

也称 (ceil(m/2), m)-Tree。

### DS

每个节点两个Vector，分别存储key，指针。![Screen Shot 2021-01-05 at 10.59.38 AM](DSA_Review.assets/Screen%20Shot%202021-01-05%20at%2010.59.38%20AM.png)

某node中，两key之间的下级指针指向的node中的key介于两key之间。

#### 最大树高

$\Omega(log_m N) \le h \le O(log_m N)$。

### Algorithm

#### Find

每一层内部顺序查找。但因为顺序局部访问很快，时间可以忽略。

当找不到时，转入下一层查找。

$O(logn)$。n为key的个数。

#### Insert

先插入v：

1. 寻找v，确认其不存在，寻找过程中记录了hot（查找失败的最后一个node的parent）。

2. 在hot里插入v。

视情况做**分裂**。

#### 上溢与split

上溢节点恰好有m个key。

![image-20210105112950326](DSA_Review.assets/image-20210105112950326.png)

上溢传递：最多传到根。如果根满了，就上溢一个元素作为新的根。

显然上溢传递次数是有界的。

$O(h) = O(log_mN)$。

#### 删除

![image-20210105115300598](DSA_Review.assets/image-20210105115300598.png)

1. 找直接后继的数：对于非叶节点需先找直接后继节点：先往右下一次，然后一路向左，直到叶节点。
2. 交换
3. 删除交换后的，需要删除的，已经在叶节点中的数

删除后需要处理下溢。

#### 下溢

下溢节点恰好有$O(ceil(m/2) - 2)$个key。

分情况：

1. （Rotate $O(1)$）左兄弟（同parent）存在，且其key个数-1仍满足条件：

![Screen Shot 2021-01-05 at 12.02.56 PM](DSA_Review.assets/Screen%20Shot%202021-01-05%20at%2012.02.56%20PM.png)

2. （Rotate $O(1)$）右兄弟（同parent）存在，且其key个数-1仍满足条件：

![Screen Shot 2021-01-05 at 12.03.07 PM](DSA_Review.assets/Screen%20Shot%202021-01-05%20at%2012.03.07%20PM.png)

3. （Combine  $O(log_mN)$）L或R不存在（不可能同时不存在，因为一个node至少两个分支），或key的个数已达下限不能再给出：

![Screen Shot 2021-01-05 at 12.07.08 PM](DSA_Review.assets/Screen%20Shot%202021-01-05%20at%2012.07.08%20PM.png)

合并出来的节点一定满足key个数的范围条件。

父节点P少了一个key，可能下溢。因此下溢会传递。

## Red Black Tree





## Heap

上大下小，或上小下大。



## Leftlist Heap

### Definition

Null path length: 

若x为外部节点，则$npl(x) = 0$；否则，$npl(x) = 1 + min(~npl(~lc(x)~), npl(~rc(x)~)~)$

$npl$的意义：**x到外部节点的最短距离**。

**左式堆**：$\forall x, npl(~lc(x)~) \ge npl(~rc(x)~)$。

即，对npl值，任意内部节点的左孩子不小于右孩子。

（然而如图，左子树的高度并不一定不小于右子树。）

![Screen Shot 2021-01-05 at 4.26.48 PM](DSA_Review.assets/Screen%20Shot%202021-01-05%20at%204.26.48%20PM.png)

于是有性质：$npl(x) = 1 + npl(rc(x))$，也等于**最右侧通路的长度d**。$d = O(logn)$。

### Algorithm

merge(a,b)：

1. 通过swap确保a>b。
2. 将$a_R$与$b$合并成新的$a_{R'}$，若新的$a_{R'}$与$a_{L}$不满足npl关系，则交换之。
3. 维护a的npl。



## KMP





## BM







