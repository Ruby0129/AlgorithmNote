### 查找

```c++
// 查找算法介绍
// 常用的查找算法
// 1.顺序(线性)查找
// 2.二分查找/折半查找
// 3.插值查找
// 4.斐波那契查找
```

### 线性查找

```c++
// 线性查找
# include<iostream>
# include<vector>
using namespace std;

int seqSearch(vector<int> arr, int value){
	// 线性查找是逐一比对，发现有相同的值时返回下标
	for(int i = 0; i < arr.size(); i++){
		if(arr[i] == value){
			return i;
		}
	}
	return -1; 
}

int main(){
	
	vector<int> arr{1,9,11,-1,34,89};
	int i = seqSearch(arr, 34); 
	if(i != -1){
		cout << "找到数"<< arr[i] << "下标为" << i << endl; 
	}else{
		cout << "没有查找到" << endl;
	}
	system("pause");
	return 0;
} 
```



### 二分查找

```c++
// 二分查找
# include<iostream>
# include<vector>
using namespace std;

// 二分查找的思路分析
/*  使用二分查找的前提是数组有序 
	1. 首先确定该数组的中间的下标mid = (left+right) / 2
	2.然后让需要查找的数findVal和arr[mid]比较
		2.1 findVal > arr[mid] 要查找的数在mid的右边，递归的向右查找
		2.2 findVal < arr[mid] 要查找的数在mid的左边，递归的向左查找
		2.3 findVal == arr[mid] 找到返回
	什么时候结束递归?
	1.找到就结束递归
	2.递归完整个数组，仍然没有找到findVal,也需要结束递归 当left > right就需要退出 
*/

// 二分查找递归实现  
int BinarySearch(vector<int> arr, int left, int right, int findVal){
	// 如果找到返回小标
	int mid = left + (right - left) / 2;
	int midVal = arr[mid]; 
	// 没有找到返回-1 
	if(left > right){
		return -1;
	}
	if(findVal > midVal){
		// 向右递归
		return BinarySearch(arr, mid+1, right, findVal); 
	}else if(findVal < midVal){
		// 向左递归 
		return BinarySearch(arr, left, mid-1, findVal); 
	}else{
		return mid;
	}
	
}

int main(){
	
	vector<int> arr{1,8,10,89,1000,1234};
	int i = BinarySearch(arr, 0, arr.size()-1, 1234); 
	if(i != -1){
		cout << "找到数:"<< arr[i] << " 下标为:" << i << endl; 
	}else{
		cout << "没有查找到" << endl;
	}
	system("pause");
	return 0;
}  
```

### 二分查找扩展

```c++
// 扩展 {1,8,10,89,1000,1000,1000,1234}当一个有序数组中，有多个相同的数值时，如何将所有的数值都查找到，比如这里的1000
/*
	思路分析:
	1. 在找到mid值时，不要马上返回
	2. 向mid索引值的左边扫描，将所有满足1000的元素的下标，加入到集合中
	3. 向mid索引值的右边扫描，将所有满足1000的元素的下标，加入到集合中 
*/ 
vector<int> BinarySearch2(vector<int> arr, int left, int right, int findVal){
	// 如果找到返回小标
	int mid = left + (right - left) / 2;
	int midVal = arr[mid]; 
	// 没有找到返回-1 
	if(left > right){
		return {};
	}
	if(findVal > midVal){
		// 向右递归
		return BinarySearch2(arr, mid+1, right, findVal); 
	}else if(findVal < midVal){
		// 向左递归 
		return BinarySearch2(arr, left, mid-1, findVal); 
	}else{
		vector<int> index;
		// 向左边扫描
		int temp = mid - 1;
		while(true){
			if(temp < 0 || arr[temp] != findVal){
				 break;
			}
			// 否则将temp放入到集合中 
			index.push_back(temp);
			temp--; 
		} 
		index.push_back(mid);
		// 向右边扫描
		temp = mid + 1;
		while(true){
			if(temp > arr.size()-1 || arr[temp] != findVal){
				break;
			}
			index.push_back(temp);
			temp++;
		}
		return index; 
	}
}
```



### 插值查找

```c++
// 插值查找
# include<iostream>
# include<vector>
using namespace std;

// 插值查找的原理介绍 
/* 1. 插值查找算法类似于二分查找，不同的是插值查找每次总自适应mid处开始查找。
   2. 将折半查找中的求mid索引的方式，变为 int mid = low + (high - low) * (key - arr[low]) / (arr[high] - arr[low]);
   	  key就是要找的findVal   
*/

// 插值查找 
int insertValueSearch(vector<int> arr, int left, int right, int findVal){
	// 注意: findVal < arr[0] || findVal > arr[arr.size() - 1]必须有，否则得到的mid可能越界 
	if(left > right || findVal < arr[0] || findVal > arr[arr.size() - 1]){
		return -1;
	}
	int mid = left + (right - left) * ((findVal - arr[left]) / (arr[right] - arr[left]));
	int midVal = arr[mid];
	cout << "mid = " << mid << endl;
	if(findVal > midVal){
		return insertValueSearch(arr, mid + 1,  right, findVal);
	}else if(findVal < midVal){
		return insertValueSearch(arr, left, mid - 1, findVal);
	}else{
		return mid;
	}
} 

inline ostream&
operator << (ostream& os, const vector<int>arr){
	for(auto elem : arr){
		cout << elem << " ";
	}
	cout << endl;
}


int main(){
	vector<int> arr(100, 0);
	for(int i = 0; i < 100; i++){
		arr[i] = i + 1;
	}
	int res = insertValueSearch(arr, 0, arr.size()-1, 100);
	if(res != -1){
		cout << "找到数:"<< arr[res] << " 下标为:" << res << endl; 
	}else{
		cout << "没有查找到" << endl;
	}
	system("pause");
	return 0;
}  
```



### 斐波那契查找

```c++

```

