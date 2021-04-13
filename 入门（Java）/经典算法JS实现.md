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

