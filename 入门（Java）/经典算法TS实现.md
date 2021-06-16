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

```tsx
const selectSort = function (nums: number[]) {
	const len = nums.length
	for (let i = 0; i < len; i++) {
		let min = i
		for (let j = i + 1; j < len; j++) {
			if (nums[j] < nums[min]) {
				min = j
			}
		}
		;[nums[i], nums[min]] = [nums[min], nums[i]]
	}
	return nums
}
```

#### 冒泡排序

```tsx
const bubbleSort = function (arrs: number[]) {
	const len = arrs.length
	let isSortFlag = false // 如果排好序只需遍历一遍
	for (let i = len - 1; i > 0 && !isSortFlag; i--) {
		isSortFlag = true
		for (let j = 0; j < i; j++) {
			if (arrs[j] > arrs[j + 1]) {
				isSortFlag = false
				;[arrs[j], arrs[j + 1]] = [arrs[j + 1], arrs[j]]
			}
		}
	}
	return arrs
}
```

#### 插入排序

```tsx
const insertSort = function (nums: number[]) {
	const len = nums.length
	for (let i = 1; i < len; i++) {
		for (let j = i; j > 0; j--) {
			if (nums[j] < nums[j - 1]) {
				;[nums[j], nums[j - 1]] = [nums[j - 1], nums[j]]
			}
		}
	}
	return nums
}
```

#### 希尔排序

```java

```

#### 归并排序

```java

```

#### 快速排序

```java

```

#### 堆排序

```java

```

