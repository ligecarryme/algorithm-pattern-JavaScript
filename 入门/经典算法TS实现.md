[TOC]

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

```tsx
const shellSort = (nums: number[]) => {
  const len = nums.length
  let h = len >> 1
  while (h >= 1) {
    for (let i = h; i < len; i++) {
      for (let j = i; j >= h && nums[j] < nums[j - h]; j -= h) {
        [nums[j], nums[j - h]] = [nums[j - h], nums[j]]
      }
    }
    h >>= 1
  }
}
```

#### 归并排序

```tsx
const mergeSort = (nums: number[], left: number, right: number) => {
  if (left === right) return [nums[left]]
  const mid = left + ((right - left) >> 1)
  const leftArray = mergeSort(nums, left, mid)
  const rightArray = mergeSort(nums, mid + 1, right)
  const newArray: number[] = new Array(leftArray.length + rightArray.length)
  let i = 0, j = 0, k = 0
  while (i < leftArray.length && j < rightArray.length) {
    newArray[k++] = leftArray[i] < rightArray[j] ? leftArray[i++] : rightArray[j++]
  }
  while (i < leftArray.length) {
    newArray[k++] = leftArray[i++]
  }
  while (j < rightArray.length) {
    newArray[k++] = rightArray[j++]
  }
  return newArray
}
```

#### 快速排序

```tsx
const quickSort = (nums: number[], start: number, end: number) => {
  let left = start
  let right = end
  const pivot = nums[start]
  while (left < right) {
    while (left < right && nums[right] > pivot) {
      right--
    }
    while (left < right && nums[left] < pivot) {
      left++
    }
    if (nums[left] === nums[right] && left < right) {
      left++
    } else {
      [nums[left], nums[right]] = [nums[right], nums[left]]
    }
  }
  if (start < left - 1) {
    quickSort(nums, start, left - 1)
  }
  if (right + 1 < end) {
    quickSort(nums, right + 1, end)
  }
  return nums
}
```

#### 堆排序

```java

```

