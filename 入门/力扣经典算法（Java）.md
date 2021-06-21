[TOC]

### 常用方法

**char数组转换为String类型**

```java
char[] chars = ['a','b','c'];  
String s = String.valueOf(chars);   //方法一
String s = new String(chars);  //方法二 new一个String
```

**String类型转char数组**

``` java
char[] chars = s.toCharArray();
```

```java
Pattern.matchs() // 字符串匹配
```

### 动态规划 

**第一步骤**：定义**数组元素的含义**，我们会用一个数组，来保存历史数组，假设用一维数组 dp[] 吧。这个时候有一个非常非常重要的点，就是规定你这个数组元素的含义，例如你的 dp[i] 是代表什么意思。

**第二步骤**：找出**数组元素之间的关系式**，我觉得动态规划，还是有一点类似于我们高中学习时的**归纳法**的，当我们要计算 dp[n] 时，是可以利用 dp[n-1]，dp[n-2].....dp[1]，来推出 dp[n] 的，也就是可以利用**历史数据**来推出新的元素值，所以我们要找出数组元素之间的关系式，例如 dp[n] = dp[n-1] + dp[n-2]，这个就是他们的关系式了。而这一步，也是最难的一步，后面我会讲几种类型的题来说。

**第三步骤**：找出**初始值**。学过**数学归纳法**的都知道，虽然我们知道了数组元素之间的关系式，例如 dp[n] = dp[n-1] + dp[n-2]，我们可以通过 dp[n-1] 和 dp[n-2] 来计算 dp[n]，但是，我们得知道初始值啊，例如一直推下去的话，会由 dp[3] = dp[2] + dp[1]。而 dp[2] 和 dp[1] 是不能再分解的了，所以我们必须要能够直接获得 dp[2] 和 dp[1] 的值，而这，就是**所谓的初始值**。

由了**初始值**，并且有了**数组元素之间的关系式**，那么我们就可以得到 dp[n] 的值了，而 dp[n] 的含义是由你来定义的，你想**求什么，就定义它是什么**，这样，这道题也就解出来了。

90%的字符串匹配问题可用动态规划解决。

### 二叉树遍历

#### 节点查找：

```java
public TreeNode preOrder(TreeNode A,TreeNode B){
        if (A==null) return null;
        if (A.val== B.val) return A;
        TreeNode tmpA,tmpB;
        if ((tmpA=preOrder(A.left,B))!=null){
            return tmpA;
        }else if((tmpB=preOrder(A.right,B))!=null){
            return tmpB;
        }else
            return null;
    }
```

#### 先序遍历

```java
public void traversePreOrder(Node node) { //递归
    if (node != null) {
        visit(node.value);
        traversePreOrder(node.left);
        traversePreOrder(node.right);
    }
}
```

```java
public void traversePreOrderWithoutRecursion() { //非递归
    Stack<Node> stack = new Stack<Node>();
    Node current = root;
    stack.push(root);
    while(!stack.isEmpty()) {
        current = stack.pop(); //先根后左子树、右子树
        visit(current.value);
        
        if(current.right != null) {
            stack.push(current.right);
        }    
        if(current.left != null) {
            stack.push(current.left);
        }
    }        
}

```

#### 中序遍历

```java
public void traverseInOrder(Node node) {//递归
    if (node != null) {
        traverseInOrder(node.left);
        visit(node.value);
        traverseInOrder(node.right);
    }
```

```java
public void traverseInOrderWithoutRecursion() {//非递归
    Stack<Node> stack = new Stack<Node>();
    Node current = root;
    stack.push(root);
    while(!stack.isEmpty()) {
        while(current.left != null) {//先左子树、然后根、右子树
            current = current.left;                
            stack.push(current);                
        }
        current = stack.pop();
        visit(current.value);
        if(current.right != null) {
            current = current.right;                
            stack.push(current);
        }
    }
}
```

#### 后序遍历

```java
public void traversePostOrder(Node node) {	//递归
    if (node != null) {
        traversePostOrder(node.left);
        traversePostOrder(node.right);
        visit(node.value);
    }
}
```

```java
public void traversePostOrderWithoutRecursion() {	//后序的递归后续理解
    Stack<Node> stack = new Stack<Node>();
    Node prev = root;
    Node current = root;
    stack.push(root);
 
    while (!stack.isEmpty()) {
        current = stack.peek();
        boolean hasChild = (current.left != null || current.right != null);
        boolean isPrevLastChild = (prev == current.right || 
          (prev == current.left && current.right == null));
 
        if (!hasChild || isPrevLastChild) {
            current = stack.pop();
            visit(current.value);
            prev = current;
        } else {
            if (current.right != null) {
                stack.push(current.right);
            }
            if (current.left != null) {
                stack.push(current.left);
            }
        }
    }   
```

#### 层次遍历

```java
public void levelIterator(BiTree root)
  {
	  if(root == null)
	  {
		  return ;
	  }
	  LinkedList<BiTree> queue = new LinkedList<BiTree>();
	  BiTree current = null;
	  queue.offer(root);	//将根节点入队
	  while(!queue.isEmpty())
	  {
		  current = queue.poll();	//出队队头元素并访问
		  System.out.print(current.val +"-->");
		  if(current.left != null)	//如果当前节点的左节点不为空入队
		  {
			  queue.offer(current.left);
		  }
		  if(current.right != null)	//如果当前节点的右节点不为空，把右节点入队
		  {
			  queue.offer(current.right);
		  }
	  }

```

#### DFS 深度搜索-从上到下

```java
public class Solution {
    public List<Integer> dfsUpToDown(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        dfs(root, res);
        return res;
    }
    
    public void dfs(TreeNode node, List<Integer> res) {
        if (node == null) {
            return;
        }
        res.add(node.val);
        dfs(node.left, res);
        dfs(node.right, res);
    }
}
```

#### DFS 深度搜索-从下向上（分治法）

```java
public class Solution {
    public List<Integer> prerderTraversal(TreeNode root) {
        return divideAndConquer(root);
    }

    public List<Integer> divideAndConquer(TreeNode node) {
        List<Integer> result = new LinkedList<>();
        if (node == null) {
            return null;
        }
        // 分治
        List<Integer> left = divideAndConquer(node.left);
        List<Integer> right = divideAndConquer(node.right);
        // 合并结果
        result.add(node.val);
      	if (left != null) {
            result.addAll(left);
        }
        if (right != null) {
            result.addAll(right);
        }
        return result;
    }
}
```

注意点：

> DFS 深度搜索（从上到下） 和分治法区别：前者一般将最终结果通过指针参数传入，后者一般递归返回结果最后合并

#### BFS 层次遍历

```java
public class Solution {
    public List<Integer> levelOrder(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()) {
            // size记录当前层有多少元素（遍历当前层，再添加下一层）
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                TreeNode node = queue.poll();
                res.add(node.val);
                if (node.left != null) {
                    queue.add(node.left);
                }
                if (node.right != null) {
                    queue.add(node.right);
                }
            }
        }
        return res;
    }
}
```

### 图遍历

#### 深度优先遍历

```java
public void dfs(int start) {
    boolean[] isVisited = new boolean[adjVertices.size()];
    dfsRecursive(start, isVisited);
}
 
private void dfsRecursive(int current, boolean[] isVisited) {//递归
    isVisited[current] = true;
    visit(current);
    for (int dest : adjVertices.get(current)) {
        if (!isVisited[dest])
            dfsRecursive(dest, isVisited);
    }
}
```

```java
public void dfsWithoutRecursion(int start) {//非递归
    Stack<Integer> stack = new Stack<Integer>();
    boolean[] isVisited = new boolean[adjVertices.size()];
    stack.push(start);
    while (!stack.isEmpty()) {
        int current = stack.pop();
        isVisited[current] = true;
        visit(current);
        for (int dest : adjVertices.get(current)) {
            if (!isVisited[dest])
                stack.push(dest);
        }
    }
}
```

#### 广度优先遍历

```java
static void BFS(Graph g){
  Queue<Integer> queue = new LinkedList<Integer>(g.VertexNum);
  int i,j;
  for(i = 0; i < g.VertexNum; i++){  //初始化顶点的访问情况
    g.isTrav[i] = 0;
  }
  for(i = 0; i < g.VertexNum; i++){  //遍历每一个顶点，实现从每一个顶点出发的广度优先遍历
    g.isTrav[i] = 1;
    System.out.println("->"+g.Vertex[i]);
    queue.offer(i);
    while(!queue.isEmpty()){  //队列空了才会跳出循环，意味着从某个顶点出发的广度优先遍历已经结束了，下次循环意味着要从另外一个顶点出发
      vertex = queue.poll();
      for(j = 0; j < g.VertexNum; j++){
         if(g.EdgeWeight[i][j] != g.MaxValue && g.isTrav[i] == 0){
          System.out.println(g.Vertex[i]);
          g.isTrav[i] = 1;
          queue.offer(i);
         }
      }
  }
```

### 贪心算法

### 递归

### 回溯

回溯法（back tracking）（探索与回溯法）是一种选优搜索法，又称为试探法，按选优条件向前搜索，以达到目标。但当探索到某一步时，发现原先选择并不优或达不到目标，就退回一步重新选择，这种走不通就退回再走的技术为回溯法，而满足回溯条件的某个状态的点称为“回溯点”。（基于深度优先搜索）

> 八皇后问题，在8X8格的国际象棋上摆放八个皇后（棋子），使其不能互相攻击，即任意两个皇后都不能处于同一行、同一列或同一斜线上。
>
> Step 1: 先尝试放置第一枚皇后，被涂黑的地方是不能放皇后

> <img src="https://upload-images.jianshu.io/upload_images/15646173-33dddd819bf7b68d.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp" alt="img" style="zoom:67%;" />

> Step 2 : 第二行的皇后只能放在第三格或第四格，比方我们放第三格，则：
>
> <img src="https://upload-images.jianshu.io/upload_images/15646173-6fbc6cf6ab2d9846.png?imageMogr2/auto-orient/strip|imageView2/2/w/357/format/webp" alt="img" style="zoom:67%;" />

> Step 3 : 可以看到再难以放下第三个皇后，此时我们就要用到**回溯**算法了。我们把第二个皇后更改位置，此时我们能放下第三枚皇后了。
>
> <img src="https://upload-images.jianshu.io/upload_images/15646173-93e03240d2ba5b3a.png?imageMogr2/auto-orient/strip|imageView2/2/w/356/format/webp" alt="img" style="zoom:67%;" />

> Step 4 : 虽然是能放置第三个皇后，但是第四个皇后又无路可走了。返回上层调用（3号皇后），而3号也别无可去，继续回溯上层调用（2号），2号已然无路可去，继续回溯上层（1号），于是1号皇后改变位置如下，继续回溯。
>
> <img src="https://upload-images.jianshu.io/upload_images/15646173-94fd1ae52ee97f8f.png?imageMogr2/auto-orient/strip|imageView2/2/w/357/format/webp" alt="img" style="zoom:67%;" />



### 快速幂

二分角度

```java
(n&1)==1 //判断n是否为奇数  1101 1110
n>>=1 	//n右移一位  1000>>1 --> 100
```

### 排序算法

|       算法       | 稳定 | 原地排序 |          时间复杂度          | 空间复杂度 |           备注           |
| :--------------: | :--: | :------: | :--------------------------: | :--------: | :----------------------: |
|     冒泡排序     | yes  |   yes    |             N^2^             |     1      |                          |
|     选择排序     |  no  |   yes    |        N<sup>2</sup>         |     1      |                          |
|     插入排序     | yes  |   yes    |      N \~ N<sup>2</sup>      |     1      | 时间复杂度和初始顺序有关 |
|     希尔排序     |  no  |   yes    | N 的若干倍乘于递增序列的长度 |     1      |                          |
|     快速排序     |  no  |   yes    |            NlogN             |    logN    |                          |
| 三向切分快速排序 |  no  |   yes    |          N \~ NlogN          |    logN    |   适用于有大量重复主键   |
|     归并排序     | yes  |    no    |            NlogN             |     N      |                          |
|      堆排序      |  no  |   yes    |            NlogN             |     1      |                          |

#### 选择排序

```java
public void swap(int[] T,int i,int j){
        int temp = T[i];
        T[i] = T[j];
        T[j] = temp;
    }

public int[] selectSort(int[] nums){//选择排序 
        for (int i=0;i<nums.length-1;i++){
            int min=i;
            for (int j=i+1;j<nums.length;j++){
                if (nums[j]<nums[min]){
                    min=j;			//选择最小的元素index
                }
            }
            swap(nums,i,min);  //交换到最前面
        }
        return nums;
    }
```

#### 冒泡排序

```java
public int[] bubbleSort(int[] nums){ 
        boolean isSort = false; //如果数组已经排好序，就只需要遍历一遍
        for (int i=nums.length-1;i>0&&!isSort;i--){ 
            isSort=true;
            for (int j=0;j<i;j++){
                if (nums[j]>nums[j+1]){//将大的元素向上冒泡
                    isSort=false;
                    swap(nums,j,j+1);
                }
            }
        }
        return nums;
    }
```

#### 插入排序

```java
public int[] insertSort(int[] nums){
        for (int i=0;i<nums.length-1;i++){
            for (int j=i+1;j>0&&nums[j]<nums[j-1];j--){ //将小元素交换到之前已经排序的数组中去
                swap(nums,j,j-1);
            }
        }
        return nums;
    }
```

#### 希尔排序

```java
public void shellSort(int[] nums){
        int N=nums.length;
        int h=1; // h=N/2  就是以1，2，4的增量进行插入排序
        while (h < N / 3) {
            h = 3 * h + 1; // 1, 4, 13, 40, ...
        }
        while (h >= 1) {
            for (int i = h; i < N; i++) { //循环内是插入排序
                for (int j = i; j >= h && nums[j]<nums[j - h]; j -= h) {
                    swap(nums, j, j - h);
                }
            }
            h = h / 3;
        }
    }
```

#### 归并排序

```java
public int[] mergeSort(int[] arr,int left,int right){
        if (left==right) return new int[] {arr[left]};
        int mid = left+(right-left)/2;
        int[] leftarr = mergeSort(arr,left,mid); //递归
        int[] rightarr = mergeSort(arr,mid+1,right);
        int[] newarr = new int[leftarr.length+rightarr.length];
        int i=0,j=0,k=0;
        while (i< leftarr.length && j< rightarr.length){
            newarr[k++] = leftarr[i]<rightarr[j]? leftarr[i++]: rightarr[j++];
        }
        while (i< leftarr.length){
            newarr[k++] = leftarr[i++];
        }
        while (j<rightarr.length){
            newarr[k++] = rightarr[j++];
        }
        return newarr;
    }
```

#### 快速排序

```java
public void quickSort(int[] arr,int start,int end){
        int left=start,right=end;
        int pivot=arr[start]; 
        while (left<right) { //两个指针
			while (arr[right]>pivot && right>left){ //从右指向左，遇到比基准小的换到前面去
                right--;
            }
            while (arr[left]<pivot && right>left){//从左指向右，遇到比基准大的换到后面去
                left++;
            }
            if (arr[left]==arr[right] && left<right){
                left++;
            }else swap(arr,left,right);
        }
        if (left-1>start) quickSort(arr,start,left-1);
        if (right+1<end) quickSort(arr,right+1,end);
    }
```

#### 堆排序

```java
public static int[] heapSort(int[] array) {
        //这里元素的索引是从0开始的,所以最后一个非叶子结点array.length/2 - 1
        for (int i = array.length / 2 - 1; i >= 0; i--) {  
            adjustHeap(array, i, array.length);  //调整堆
        }
        // 上述逻辑，建堆结束
        // 下面，开始排序逻辑
        for (int j = array.length - 1; j > 0; j--) {
            // 元素交换,作用是去掉大顶堆
            // 把大顶堆的根元素，放到数组的最后；换句话说，就是每一次的堆调整之后，都会有一个元素到达自己的最终位置
            swap(array, 0, j);
            // 元素交换之后，毫无疑问，最后一个元素无需再考虑排序问题了。
            // 接下来我们需要排序的，就是已经去掉了部分元素的堆了，这也是为什么此方法放在循环里的原因
            // 而这里，实质上是自上而下，自左向右进行调整的
            adjustHeap(array, 0, j);
        }
        return array;
    }
    /**
    * 整个堆排序最关键的地方
    * @param array 待组堆
    * @param i 起始结点
    * @param length 堆的长度
    */
    public static void adjustHeap(int[] array, int i, int length) {
        // 先把当前元素取出来，因为当前元素可能要一直移动
        int temp = array[i];
        for (int k = 2 * i + 1; k < length; k = 2 * k + 1) {  //2*i+1为左子树i的左子树(因为i是从0开始的),2*k+1为k的左子树
            // 让k先指向子节点中最大的节点
            if (k + 1 < length && array[k] < array[k + 1]) {  //如果有右子树,并且右子树大于左子树
                k++;
            }
            //如果发现结点(左右子结点)大于根结点，则进行值的交换
            if (array[k] > temp) {
                swap(array, i, k);
                // 如果子节点更换了，那么，以子节点为根的子树会受到影响,所以，循环对子节点所在的树继续进行判断
                    i  =  k;
                        } else {  //不用交换，直接终止循环
                break;
            }
        }
    }
```

#### 桶排序

```java

```

### 链表

#### 头插法

#### 尾插法