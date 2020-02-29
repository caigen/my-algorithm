## Sort

```c++
#ifndef _SORT_H
#define _SORT_H

/*
 * Exchange based sort.
 */

void swap(int& a, int& b);

// 冒泡排序关键思想：交换相邻元素对直至有序。
// Bubble sort key idea: swap adjacent pairs until sorted.
void bubble_sort(int* data, int size);
void bubble_sort_for_loop(int* data, int size);

// 快速排序关键思想：选择某元素，划分，递归。
// Quick sort key idea: pick partition and recursively apply.
void quick_sort(int* data, int size);

// Hoare quick sort: [low, pivot_index], [pivot_index + 1, high]
// Hoare partition: the pivot data is not at the location returned
void quick_sort(int* data, int low, int high);
int quick_sort_partition(int* data, int low, int high);

// Lomuto version: pivot is the one. swap the smaller items to the left.
void quick_sort_lomuto(int* data, int size);
void quick_sort_lomuto(int* data, int low, int high);
int quick_sort_lomuto_partition(int* data, int low, int high);

/*
 * Selection based sort.
 */
// 选择排序关键思想：选择最小的，放在最左边
// Selection sort key idea: select the smallest, put it on the leftmost
void selection_sort(int* data, int size);

// 堆排序关键思想：建堆，选择最大的，放在最右边，维护堆
// Heap sort key idea: build the heap, select the largest, put it on the rightmost, maintain the heap
void heap_sort(int* data, int size);
void build_max_heap(int* data, int size);
void max_heapify(int* data, int i, int size);

/*
 * Insertion based sort.
 */
// 插入排序关键思想：找到元素位置，插入
// Insertion sort key idea: find the position, insert the item.
void insertion_sort(int* data, int size);

/*
 * Merge based sort.
 */
// 归并排序关键思想：划分，合并有序的两部分
// Merge sort key idea: divide, merge the sorted parts.
void merge_sort(int* data, int size);
void merge_sort(int* data, int left, int right, int* temp);
void merge(int* data, int left, int middle, int right, int* temp);

#endif // _SORT_H
```

```c++
#include "sort.h"
#include <iostream>

static void output(int* data, int size)
{
	for (int i = 0; i < size; ++i)
	{
		std::cout << data[i]
			<< ((i < size - 1) ? " " : "\n");
	}
}

void swap(int& a, int& b)
{
	int temp = a;
	a = b;
	b = temp;
}

void bubble_sort(int* data, int size)
{
	if (!data || size == 0)
	{
		return;
	}

	bool swapped = false;
	int n = size;
	do
	{
		swapped = false;

		for (int i = 1; i <= n - 1; ++i)
		{
			if (data[i - 1] > data[i])
			{
				swap(data[i - 1], data[i]);
				swapped = true;
			}
		}

		// the last is in order.
		n = n - 1;
	} while (swapped);
}

void bubble_sort_for_loop(int* data, int size)
{
	if (!data || size == 0)
	{
		return;
	}

	for (int pass = 1; pass <= size - 1; ++pass)
	{
		int n = size - pass;
		bool swapped = false;
		for (int i = 1; i < n; ++i)
		{
			if (data[i - 1] > data[i])
			{
				swap(data[i - 1], data[i]);
				swapped = true;
			}
		}

		if (!swapped)
		{
			break;
		}
	}
}

void quick_sort(int* data, int size)
{
	quick_sort(data, 0, size - 1);
}

void quick_sort(int* data, int low, int high)
{
	if (low < high)
	{
		int pivot_index = quick_sort_partition(data, low, high);
		quick_sort(data, low, pivot_index);
		quick_sort(data, pivot_index + 1, high);
	}
}

int quick_sort_partition(int* data, int low, int high)
{
	// pivot can't be the high one!
	int pivot = data[(low + high) / 2];
	int i = low - 1;
	int j = high + 1;

	while (true)
	{
		do
		{
			i = i + 1;
		} while (data[i] < pivot);

		do
		{
			j = j - 1;
		} while (data[j] > pivot);

		if (i >= j)
		{
			return j;
		}

		swap(data[i], data[j]);
	}
}

void quick_sort_lomuto(int* data, int size)
{
	quick_sort_lomuto(data, 0, size - 1);
}

void quick_sort_lomuto(int* data, int low, int high)
{
	if (low < high)
	{
		int pivot_index = quick_sort_lomuto_partition(data, low, high);
		quick_sort_lomuto(data, low, pivot_index - 1);
		quick_sort_lomuto(data, pivot_index + 1, high);
	}
}

int quick_sort_lomuto_partition(int* data, int low, int high)
{
	int pivot = data[high];

	// all the smaller items are swapped to left!
	int i = low;
	for (int j = low; j <= high - 1; ++j)
	{
		if (data[j] < pivot)
		{
			swap(data[i], data[j]);
			i = i + 1;
		}
	}

	swap(data[i], data[high]);

	return i;
}

void selection_sort(int* data, int size)
{
	for (int pass = 0; pass < size - 1; pass++)
	{
		int min_index = pass;
		for (int i = pass + 1; i < size; ++i)
		{
			if (data[i] < data[min_index])
			{
				min_index = i;
			}
		}

		if (min_index != pass)
		{
			swap(data[pass], data[min_index]);
		}
	}
}

void heap_sort(int* data, int size)
{
	build_max_heap(data, size);

	for (int i = size - 1; i >= 1; --i)
	{
		swap(data[0], data[i]);

		max_heapify(data, 0, i);
	}
}

void build_max_heap(int* data, int size)
{
	for (int i = size / 2; i >= 0; --i)
	{
		max_heapify(data, i, size);
	}
}

void max_heapify(int* data, int i, int size)
{
	int left = i * 2 + 1;
	int right = i * 2;
	int largest = i;
	if ((left < size) && (data[left] > data[i]))
	{
		largest = left;
	}
	if ((right < size) && (data[right] > data[i]))
	{
		largest = right;
	}

	if (largest != i)
	{
		swap(data[i], data[largest]);
		max_heapify(data, largest, size);
	}
}

void insertion_sort(int* data, int size)
{
	int i = 1;
	while (i < size)
	{
		int current_data = data[i];
		int j = i - 1;
		while (j >= 0 && data[j] > current_data)
		{
			data[j + 1] = data[j];
			j = j - 1;
		}
		data[j + 1] = current_data;
		
		i = i + 1;
	}
}

void merge_sort(int* data, int size)
{
	int* temp = (int*)malloc(size * sizeof(int));

	merge_sort(data, 0, size - 1, temp);

	free(temp);
}

void merge_sort(int* data, int left, int right, int* temp)
{
	// sorted when only one
	if (left >= right)
	{
		return;
	}

	int middle = (left + right) / 2;
	merge_sort(data, left, middle, temp);
	merge_sort(data, middle + 1, right, temp);
	merge(data, left, middle, right, temp);
}

void merge(int* data, int left, int middle, int right, int* temp)
{
	int i = left;
	int j = middle + 1;
	int k = left;
	while (i <= middle && j <= right)
	{
		if (data[i] < data[j])
		{
			temp[k] = data[i];
			i++;
		}
		else
		{
			temp[k] = data[j];
			j++;
		}

		k++;
	}

	while (i <= middle)
	{
		temp[k] = data[i];
		i++;
		k++;
	}

	while (j <= right)
	{
		temp[k] = data[j];
		j++;
		k++;
	}

	// copy back
	k = left;
	while (k <= right)
	{
		data[k] = temp[k];
		k++;
	}
}
```

