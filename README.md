#十大排序算法（C++实现）

## 头文件

```
	#include <iostream>
	#include <vector>
	#include <algorithm>
	#include <cmath>
	using namespace std;
```
## 打印数组
```
void sortprint(vector<int>& vec) {
	int len = vec.size();
	for (auto& a:vec) {
		cout << a << endl;
	}
	return;
}
```
## 1、冒泡排序
```
void BubbleSort(vector<int>& vec) {
	int len = vec.size();
	bool flag = true;
	for (int i = 0; i < len; i++) {
		for (int j = 0; j < len - i-1; j++) {
			if (vec[j] > vec[j + 1]) {
				swap(vec[j], vec[j+1]);
				flag = false;
			}
		}
		if (flag)
			return;
	}
	return;
}
```
## 2、选择排序
```
void SelectionSort(vector<int>& vec) {
	int len = vec.size();
	bool flag = true;
	for (int i = 0; i < len; i++) {
		int minIndex = i;
		for (int j = i + 1; j < len; j++) {
			if (vec[j] < vec[minIndex]) {
				minIndex = j;
				flag = false;
			}
		}
		swap(vec[i], vec[minIndex]);
		if (flag)
			return;
	}
	return;
}
```
## 3、插入排序
```
void InsertionSort(vector<int>& vec) {
	int len = vec.size();
	for (int i = 1; i < len; i++) {
		int preIndex = i - 1, current = i;
		for (int j = i - 1; j >= 0; j--) {
			if (vec[j] > vec[current]) {
				swap(vec[j], vec[current]);
				current--;
			}
			else
				break;
		}
	}
	return;
}
```
## 4、希尔排序
```
void ShellSort(vector<int>& vec) {
	int len = vec.size();
	for (int gap = len / 2; gap > 0; gap /= 2) {
		for (int i = gap; i < len; i++) {
			int j = i,current = vec[i];
			while (j - gap >= 0 && current < vec[j - gap]) {
				vec[j] = vec[j - gap];
				j = j - gap;
			}
			vec[j] = current;
		}
	}
	return;
}
```
## 5、归并排序
```
void mergevec(vector<int>& vec, int left, int right) {
	if (left >= right)
		return;
	int mid = (left + right) / 2;
	mergevec(vec, left, mid);
	mergevec(vec, mid+1, right);
	vector<int> vv(vec.begin() + left, vec.begin() + mid+1);
	int m = 0, n = mid+1;
	int len = right - left;
	for (int i = left; i < right; i++) {
		if (n  > right) {
			vec[i] = vv[m];
			m++;
			continue;
		}
		if (vv[m] > vec[n]) {
			vec[i] = vec[n];
			n++;
		}
		else {
			vec[i] = vv[m];
			m++;
		}
	}
	if (n > right)
		vec[right] = vv[m];
	return;
}

void MergeSort(vector<int>& vec){
	int len = vec.size();
	if (len < 2) {
		return;
	}
	mergevec(vec, 0, len-1);
	return ;
}
```
## 6、快速排序
```
void quick(vector<int>& vec, int left, int right) {
	if (left >= right)
		return;
	int temp = vec[left], start = left, end = right, key = left;
	while (start < end) {
		while (vec[end] > temp && start<end) {
			end--;
		}
		swap(vec[end], vec[start]);
		while (vec[start] < temp && start <end) {
			start++;
		}
		swap(vec[start], vec[end]);
	}
	quick(vec, left, start);
	quick(vec, start + 1, right);
	return;
}

void QuickSort(vector<int>& vec) {
	int len = vec.size();
	if (len < 2)
		return;
	quick(vec, 0, len - 1);
	return;
}
```
## 7、最大堆排序
```
void maxheap(vector<int>& vec, int left, int right) {
	int dad = left;
	int son = dad * 2 + 1;
	while (son <= right) {
		if (son + 1 <= right && vec[son] < vec[son+1])
			son++;
		if (vec[dad] > vec[son])
			return;
		else {
			swap(vec[dad], vec[son]);
			dad = son;
			son = dad * 2 + 1;
		}
	}
}

void HeapSort(vector<int>& vec) {
	int len = vec.size();
	if (len < 2)
		return;
	for (int i = len/2 - 1; i >= 0; i--) {
		maxheap(vec, i, len - 1);
	}

	for (int i = len-1; i > 0 ; i--) {
		swap(vec[0], vec[i]);

		maxheap(vec, 0, i-1);
	}
	return;
}
```
## 8、计数排序
```
void CountingSort(vector<int>& vec) {
	int len = vec.size();
	int max_num = INT_MIN;
	for (int i = 0; i < len; i++) {
		max_num = max(max_num, vec[i]);
	}
	vector<int> vv(max_num + 1,0);
	for (int i = 0; i < len; i++) {
		vv[vec[i]] += 1;
	}
	int j = 0;
	for (int i = 0; i <= max_num; i++) {
		if (vv[i] != 0) {
			vec[j] = i;
			j++;
			vv[i]--;
			i--;
		}
	}
	return;
}
```
## 9、桶排序
```
void BucketSort(vector<int>& vec) {
	int len = vec.size();
	int min_num = INT_MAX, max_num = INT_MIN;
	for (int i = 0; i < len; i++) {
		min_num = min(min_num, vec[i]);
		max_num = max(max_num, vec[i]);
	}
	int bucketsize = 5;
	vector<vector<int>> vv(5, vector<int>());
	for (int i = 0; i < len; i++) {
		vv[(vec[i] - min_num) / bucketsize].push_back(vec[i]);
	}
	int j = 0;
	for (int i = 0; i < bucketsize; i++) {
		sort(vv[i].begin(), vv[i].end());
		int temp = j;
		for (; j < temp + vv[i].size(); j++) {
			vec[j] = vv[i][j - temp];
		}
	}
	return;
}
```
## 10、基数排序
```
void RadixSort(vector<int>& vec) {
	int len = vec.size();
	int max_num = INT_MIN;
	for (int i = 0; i < len; i++) {
		max_num = max(max_num, vec[i]);
	}
	int bit = 0;
	while (max_num / 10 > 0) {
		max_num /= 10;
		bit++;
	}
	bit++;
	for (int i = 0; i < bit; i++) {
		vector<vector<int>> vv(10, vector<int>());
		for (int j = 0; j < len; j++) {
			int ppp = pow(10, i + 1);
			int temp = vec[j] % ppp;
			temp /= pow(10,i);
			vv[temp].push_back(vec[j]);
		}
		int j = 0;
		for (int i = 0; i < 10; i++) {
			int temp = j;
			for (; j < temp + vv[i].size(); j++) {
				vec[j] = vv[i][j - temp];
			}
		}
	}
	return;
}
```
## 主函数
```
int main() {
	vector<int> vec;
	for (int i = 0; i < 10; i++) {
		vec.push_back(10 - i);
		//vec.push_back(10 - i);
	}
	cout << "--------------------排序前-------------------------" << endl;
	sortprint(vec);
	//BubbleSort(vec);
	//SelectionSort(vec);
	//InsertionSort(vec);
	//ShellSort(vec);
	//MergeSort(vec);
	//QuickSort(vec);
	//HeapSort(vec);
	//CountingSort(vec);
	//BucketSort(vec);
	RadixSort(vec);
	cout << "--------------------排序后-------------------------" << endl;
	sortprint(vec);
	return 0;
}
```