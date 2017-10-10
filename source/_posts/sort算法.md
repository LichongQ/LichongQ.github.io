---
title: sort算法
date: 2017-09-29 22:30:24
tags: sort算法
---

常用的排序算法

- 冒泡排序(Bubble Sort)
- 选择排序(Selection Sort)
- 插入排序(Insert Sort)
- 快速排序(Quick Sort)
- 归并排序(Merge Sort)
- 希尔排序(Shell Sort)

<!--more-->


# 冒泡排序(Bubble Sort)

## 算法思想

冒泡排序是通过相邻元素间的比较和交换，来逐步将最大值/次最大值/...等不断地筛选出来。

## 代码实现

```

	func BubbleSort(array []int) error {

		if array == nil || len(array) <= 0 {
			return fmt.Errorf("array is nil.")
		}
	
		for i := 0; i < len(array); i++ {
			for j := 0; j < len(array)-i-1; j++ {
				if array[j] > array[j+1] {
					swap(&array[j], &array[j+1])
				}
			}
		}
	
		return nil
	}

```

# 选择排序(Select Sort)

## 算法思想

选择排序是通过当前元素和剩余的其他元素间的比较和交换，来筛选出最小值/次小值/...等，并放到当前元素所在的位置。

## 代码实现

```
	
	for index := 0; index < len(array)-1; index++ {
		for indexJ := index + 1; indexJ < len(array); indexJ++ {
			if array[indexJ] < array[index] {
				swap(&array[indexJ], &array[index])
			}
		}
	}
```

# 插入排序(Insert Sort)

## 算法思想

插入排序是通过将当前元素不断插入之前已经有序的数列中，来实现数据的排序。

## 代码实现

```

	for index := 1; index < len(array); index++ {
		temp := array[index]
		for indexJ := index - 1; indexJ >= 0; indexJ-- {
			if array[indexJ] <= temp {
				break
			}
			swap(&array[indexJ], &array[indexJ+1])
		}
	}
```

# 快速排序(Quick Sort)

## 算法思想

快速排序是不断地将数组进行分块，并且每次都将中枢值放在正确的位置来实现数据的排序。

## 代码实现

```

	func QuickSort(array []int) error {
		if array == nil || len(array) <= 0 {
			return fmt.Errorf("array is nil.")
		}
		quickSort(array, 0, len(array)-1)
		return nil
	}
	
	func quickSort(array []int, start int, end int) {
	
		// partition
		pivot := array[end]
		pivotIndex := start
	
		for index := start; index < end; index++ {
			if array[index] < pivot {
				swap(&array[index], &array[pivotIndex])
				pivotIndex++
			}
		}
	
		// put pivot in right place
		swap(&array[pivotIndex], &array[end])
	
		// recur
		if pivotIndex > start+1 {
			quickSort(array, start, pivotIndex-1)
		}
		if pivotIndex+1 < end {
			quickSort(array, pivotIndex+1, end)
		}
	
	}
```

# 归并排序(Merge Sort)

## 算法思想

归并排序是将数据逐个划分成只含有一个元素的小块，通过将小块不断地合并（插入排序方式合并），最终实现数据的排序。

## 代码实现

```

	func MergeSort(array []int) error {
		if array == nil || len(array) <= 0 {
			return fmt.Errorf("array is nil.")
		}
		mergeSort(array, 0, len(array)-1)
		return nil
	}
	
	func mergeSort(array []int, start int, end int) {
		// recur to split
		midleIndex := (start + end) / 2
		if start < midleIndex {
			mergeSort(array, start, midleIndex)
		}
		if midleIndex+1 < end {
			mergeSort(array, midleIndex+1, end)
		}
		// merge by insert sort
		for index := midleIndex; index <= end; index++ {
			temp := array[index]
			for indexJ := index - 1; indexJ >= start; indexJ-- {
				if array[indexJ] <= temp {
					break
				}
				swap(&array[indexJ], &array[indexJ+1])
			}
		}
	}
```

# 希尔排序(Shell Sort)

## 算法思想

希尔排序是插入排序的一种变体，给定跨度，按跨度在数据中取出相应的数据来进行插入排序，并通过不断地缩小跨度来实现数据的排序。

跨度初始值一般为length/2,以后每次减小为原来的一半，直至跨度等于1，完成数据的排序。

## 代码实现

```

	for step := len(array) / 2; step > 0; step = step / 2 {
		for index := step; index < len(array); index += step {
			temp := array[index]
			for indexJ := index - step; indexJ >= 0; indexJ -= step {
				if array[indexJ] < temp {
					break
				}
				swap(&array[indexJ], &array[indexJ+step])
			}
		}
	}
```