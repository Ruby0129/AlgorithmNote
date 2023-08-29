### [排序]()

```c++
// 排序也称排序算法
// 排序分类
/*
	1.内部排序
		指将需要处理的所有数据都加载到内部存储器中进行排序
		插入排序:直接插入排序 希尔排序
		选择排序:简单选择排序 堆排序
		交换排序:冒泡排序 快速排序
		归并排序
		基数排序
	2.外部排序(适合内存和外存结合)
		数据量过大，无法全部加载到内存中，需要借助外部存储进行排序。
*/
```

```c++
// 时间复杂度
/*
	1. 事后统计法
	需要先运行程序，非常耗费时间。
	一个程序的时间可能依赖于计算机硬件、软件等环境因素，这种状态下，需要在同一台计算机的相同状态下运行，才能比较哪个算法速度比	 较快。
	
	2.事前估算的方法(时间复杂度)
*/

// 常见的时间复杂度
/*
1.常数阶O(1)
2.对数阶O(logn)
3.线性阶O(n)
4.线性对数阶O(nlogn)
5.平方阶O(n^2)
6.立方阶O(n^3)
7.k次方阶O(n^k)
8.指数阶O(2^n)
9.O(n!)
*/
```

### 冒泡排序

```c++
// 第一趟排序就是将最大的数排在最后
void bubbleSort(vector<int> &arr){
	
	int temp = arr[0];
	bool flag = false; // 表示变量，表示是否发生过交换 
	for(int i=0;i<arr.size();i++){
		for(int j=0;j<arr.size()-i-1;j++){
			if(arr[j] > arr[j+1]){
				flag = true;				
				temp = arr[j];
				arr[j] = arr[j+1];
				arr[j+1] = temp;				
			}
		}
		if(flag == false){
			// 没有发生过交换
			break; 
		}else{
			flag = false; //重置flag进行下次判断 
		}
	}
	
} 
```

### 选择排序

```c++
//  1.选择排序一共有数组大小-1轮排序 
//  2.每一轮排序，又是一个循环
// 循环的规则
/*
	1.先假定当前这个数是最小数
	2.然后和后面的每个数比较，如果发现有比当前数更小的数，就重新确定最小数，并确定下标
	3.当遍历到数组的最后时，就将本轮最小小标的数和最小数交换 
*/ 
// 选择排序复杂度是O(n^2)
void swap(int &num1, int &num2){
	// 交换函数 
	int temp = num1;
	num1 = num2;
	num2 = temp;
} 

void selectSort(vector<int>&array){
	
	int min = array[0];
	// 存储最小值的下标 
	int min_index = 0;
	
	for(int i = 0; i<array.size() - 1;i++){
		min = array[i];
		min_index = i;
		for(int j = i+1; j < array.size();j++){
			if(min > array[j]){
				min = array[j];
				min_index = j;
			}
		}
		swap(array[i], array[min_index]);
	}
	// 交换最小值和当前最小小标的数	
}
```

### 插入排序

```c++
// 插入排序
#include<iostream>
#include<vector>
using namespace std;


// 插入排序法思想
/*
	把n个待排序的元素看成为一个有序表和一个无序表，开始时有序表中只包含一个元素，
	无序表中包含由n-1个元素，排序过程中每次从无序表中取出第一个元素，把它的排序码
	依次与有序表元素的排序码比较，将它插入到有序表中的适当位置，称为新的有序表 
	插入排序的时间复杂度为O(n^2)
*/ 

void InsertSort(vector<int> &arr){
	
	// {101, 34, 119, 1}
	// 第一轮 {34, 101, 119, 1}
	// 定义待插入的数
	int insertVal = 0;
	// 定义待插入的索引
	int insertIndex = 0; // 即arr[i]的前面的数的下标
	// 给insertVal找到插入的位置
	// 保证给insert找插入位置时不越界 
	// 待插入数还没有找到适当的位置 
	// 将insertIndex后移
	for(int i=1; i<arr.size(); i++){
		insertVal = arr[i];
		insertIndex = i - 1;
		while(insertIndex >= 0 && insertVal < arr[insertIndex]){
		arr[insertIndex+1] = arr[insertIndex];
		// {101, 101, 119, 1}
		insertIndex--;
	}
	if(insertIndex + 1 != i){
		arr[insertIndex + 1] = insertVal; 	
	} 
	// 当推出while循环时，插入的位置找到， insertIndex+1
	} 	
} 

inline ostream&
operator <<(ostream &os, vector<int> arr){
	
	for(int i = 0; i < arr.size(); i++){
		
		cout<<arr[i]<<" ";
		
	}
	
	cout<<endl;
	
}


int main(){
	
	vector<int> arr = {101, 34, 119, 1};
	cout<<"排序前:"<<endl;
	cout<<arr<<endl; 
	cout<<"排序后"<<endl;
	InsertSort(arr);
	cout<<arr<<endl;
	system("pause");
	return 0;
} 
```

### 希尔排序

![](D:\CandCpp\note\数据结构与算法\img\排序\希尔排序.PNG)

```c++
// 希尔排序
#include<iostream>
#include<vector>
using namespace std;

/*
	简单插入排序存在的问题
	当需要插入的数是较小的数的时候，后移的次数明显增多，对效率有影响 
	
	希尔排序基本思想
	希尔排序是把记录按下标的一定增量分组，对每组使用直接插入排序算法排序
	随着增量逐渐减少，每组包含的关键词越来越多，当增量减至1时，整个文件恰被分成一组，算法便终止 
*/ 
inline ostream&
operator <<(ostream &os, vector<int> arr){
	
	for(int i = 0; i < arr.size(); i++){
		
		cout<<arr[i]<<" ";
		
	}
	
	cout<<endl;
	
}

void Swap(int &num1, int &num2){
	
	int temp = num1;
	num1 = num2;
	num2 = temp;
	
}

// 希尔排序交换式实现 
void ShellSort_Swap(vector<int> arr){	
	
	// 希尔排序的第一轮排序
	// 因为第一轮排序，是将10个数据分成五组 
	// 第二轮排序，5 / 2 = 2组
	// 第三轮排序 2 / 2 = 1组
	int count = 0; 
	for(int gap = arr.size()/2; gap > 0; gap/=2){
		for(int i = gap; i < arr.size(); i++){
		// 遍历各组中所有的元素 (共gap组， 每组)， 步长gap 
			for(int j = i - gap; j >=0; j-=gap){
				// 下标 和 增量为5的比 
				// 如果当前元素大于加上步长后的那个元素，说明交换
				if(arr[j] > arr[j+gap]){
					Swap(arr[j], arr[j+gap]);				
				}
			}
		}
		count++;
		cout<<"希尔排序第"<<count<<"轮排序结果:"<<endl;
		cout<<arr;	
	}  	
} 

// 希尔排序移位式实现
// 对上述进行优化 
void ShellSort_move(vector<int> arr){	
	
	// 希尔排序的第一轮排序
	// 因为第一轮排序，是将10个数据分成五组 
	// 第二轮排序，5 / 2 = 2组
	// 第三轮排序 2 / 2 = 1组
	int count = 0; 
	for(int gap = arr.size()/2; gap > 0; gap/=2){		
		// 从第gap个元素开始，逐个对其所在的组进行直接插入排序
		for(int i=gap; i<arr.size();i++){			
			int j = i;
			int temp = arr[j];
				while(j - gap >= 0 && temp < arr[j - gap]){
					// 移动
					arr[j] = arr[j - gap];
					j -= gap; 
				}
				// 退出循环后，找到了temp插入的位置
				arr[j] = temp; 			
		}
		count++;
		cout<<"希尔排序第"<<count<<"轮排序结果:"<<endl;
		cout<<arr;	   		
	}	
}



int main(){
	
	vector<int> arr = {8,9,1,7,2,3,5,4,6,0,10};
	cout<<"排序前:"<<endl;
	cout<<arr<<endl; 
	cout<<"希尔排序交换法:"<<endl;
	ShellSort_Swap(arr);
	cout<<"---------------------"<<endl; 
	cout<<"希尔排序移动法:"<<endl; 
	ShellSort_move(arr);
	system("pause");
	return 0;
} 
```

### 快速排序(重要)

```c++
// 快速排序
#include<iostream>
#include<vector>
using namespace std;

inline ostream&
operator<<(ostream &os, vector<int>array){
	
	for(auto elem : array){
		cout << elem << " "; 
	}
	cout << endl;
}

int swap(int &num1, int &num2){
	int temp = num1;
	num1 = num2;
	num2 = temp;
}

// 快速排序
/*
	快速排序(QuickSort)是对冒泡排序的一种改进。基本思想是:通过一趟排序将要排序的数据分割成独立的两部分，
	其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序。
	整个排序过程可以递归进行，以此达到整个数据变成有序序列。	 
*/ 

void QuickSort(vector<int> &array, int left, int right){
	int l = left; // 左索引 
	int r = right;	// 右索引
	int pivot = array[left + (right - left)/2];// 中轴
	// while循环的目的是让比pivot小的值放到左边，大的值放到右边 
	while(l < r){
		while(array[l] < pivot){
			// 在pivot左边一直找，找到一个大于等于pivot的值，才退出 
			l += 1;
		}
		while(array[r] > pivot){
			// 在pivot右边一直找，找到一个小于等于pivot的值，才退出
			r -= 1; 
		}
		// 如果l >= r说明pivot的左右两边的值
		// 已经按照左边全部是小于等于pivot的值 右边全部是大于等于pivot的值  
		if(l >= r){
			break;
		} 
		// 交换 
		swap(array[l], array[r]); 
		// 如果交换完后，发现这个arr[l] == pivot值相等 r--, 前移 
		if(array[l] == pivot){
			r -= 1;
		}
		// 如果交换完后，发现这个arr[r] == pivot值相等 l++, 后移 
		if(array[r] == pivot){
			l += 1;
		} 
	}
	// 如果 l == r, 必须l++, r--, 否则会出现栈溢出
	if(l == r){
		l += 1;
		r -= 1; 
	}
	// 向左递归
	if(left < r){
		QuickSort(array, left, r);	
	}
	if(right > l){
		QuickSort(array, l, right);	
	}  
} 




int main(){
	
	vector<int> array{-9,78,0,23,-567,70};
	
	QuickSort(array, 0, array.size()-1);
	cout << array << endl;
	
	system("pause");
	
	return 0;
} 
```

### 归并排序

```c++
// 归并排序
// 分治思想 将问题分解为小的问题 递归求解
#include<iostream>
#include<vector>
using namespace std;

inline ostream&
operator<<(ostream &os, vector<int>array){
	
	for(auto elem : array){
		cout << elem << " "; 
	}
	cout << endl;
}

int swap(int &num1, int &num2){
	int temp = num1;
	num1 = num2;
	num2 = temp;
}

// 合并的方法
/*
	arr 排序的原始数组
	left 左边有序序列的初始索引
	mid 中间索引
	right 右边索引
	temp 做中转的数组 
*/
void Merge(vector<int> &arr, int left, int mid, int right, vector<int> &temp){
	int i = left; // 初始化i，左边有序序列的初始索引
	int j = mid + 1; // 初始化j, 右边有序序列的初始索引
	int t = 0; // t是指向temp数组的当前索引
	// 1.先把左右两边(有序)的数据按照规则填充到temp数组
	// 直到左右两边的有序序列，有一边处理完毕
	while(i <= mid && j <= right){
	 	// 继续
		 if(arr[i] <= arr[j]){
		 	// 如果左边的有序序列的当前元素，小于等于右边有序序列的当前元素
			// 就将左边的当前元素拷贝到temp数组 
		 	temp[t] = arr[i];
		 	t += 1;
		 	i += 1;
		 }else{
		 	// 反之
		 	temp[t] = arr[j];
		 	t += 1;
		 	j += 1;
		 } 
	 }
	// 2.把剩余数据的一边的数据依次全部填充到temp
	while(i <= mid){
		// 左边的有序序列还有剩余元素，就全部填充到temp
		temp[t] = arr[i];
		t += 1;
		i += 1; 
	}
	while(j <= right){
		temp[t] = arr[j];
		t += 1;
		j += 1;
	} 
	// 3.将temp数组的元素拷贝到array
	// 注意并不是每次都拷贝所有的
	t = 0;
	int tempLeft = left;
	// 第一次合并tempLeft = 0, right = 1 // tempLeft = 2 right =3  // tempLeft = 0 right = 3
	// 最后一次 tempLeft = 0 right = 7 
	while(tempLeft <= right){
		arr[tempLeft] = temp[t];
		t += 1;
		tempLeft += 1;
	} 
	cout << arr << endl;
} 

// 分+合的方法
void MergeSort(vector<int> &arr, int left, int right, vector<int> &temp){
	
	if(left < right){
		int mid = left + (right - left) / 2; // 中间索引
		// 向左递归进行分解
		MergeSort(arr, left, mid, temp);
		// 向右递归进行分解
		MergeSort(arr, mid+1, right, temp);
		// 到合并
		Merge(arr, left, mid, right, temp); 
	}
	
	
} 

int main(){
	
	vector<int> array{8,4,5,7,1,3,6,2};
	// 归并排序需要额外的空间 
	cout << "归并排序前:" <<endl;
	cout << array <<endl;
	vector<int> temp(array.size());
	cout << "归并排序后: " << endl; 
	MergeSort(array, 0, array.size()-1, temp);
	cout << array << endl;
	system("pause");
	
	return 0;
}  
```



### 基数排序

```c++
#include<iostream>
#include<string> 
#include<vector>
using namespace std;
// 基数排序
// 1.基数排序(radix sort)属于"分配式排序"(distribution sort), 又称"桶子法"(bucket sort)或bin sort。
// 	它是通过键值的各个位的值，将要排序的元素分配至某些"桶"中，达到排序的作用。
// 2.基数排序属于稳定性、效率高的排序法
// 3.基数排序是桶排序的扩展
// 4.基数排序:将整数按位数切割成不同的数字，然后按每个位数分别比较 

// 基数排序基本思想
/*
	将所有待比较数值统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。
	这样从最低为排序一直到最高位排序完成以后，数列就变成了一个有序序列 
*/ 


inline ostream&
operator<<(ostream &os, vector<int>array){
	
	for(auto elem : array){
		cout << elem << " "; 
	}
	cout << endl;
}

int swap(int &num1, int &num2){
	int temp = num1;
	num1 = num2;
	num2 = temp;
}

int FindMax(vector<int> &arr){
	int max = arr[0];
	for(auto num : arr){
		if(num > max){
			max = num;
		}
	}
	return max;
}

void RadixSort(vector<int> &arr){
	// 第一轮排序
	// 1.将每个元素的个位数取出，然后看这个数应该放在哪个对应的桶(一个一维数组)
	// 2.按照这个桶的顺序(一维数组的下标依次取出数据，放入原来数组)
	
	// 定义一个二维数组表示10个桶，每个桶就是一个一维数组
	// 1.二维数组包含10个一维数组
	// 2.为了防止在放入数的时候数据溢出，则每个一维数组，大小arr.size()
	// 空间换时间的经典算法 
	vector<vector<int>>bucket(10, vector<int>(arr.size(), 0)); 
	// 为了记录每个桶中，实际存放了多少个数据，定义一个一维数组来记录各个桶每次放入的数据个数
	vector<int> bucketElementCounts(10, 0);
	// 1. 得到数组中最大的数的位数
	int max = FindMax(arr);
	int maxLength =  to_string(max).size();
	// 使用循环将代码处理 
	for(int i=0, n=1; i<maxLength; i++, n*=10){
		// 针对每个元素的对应位数进行排序处理 个 十 百 
		for(int j = 0; j < arr.size(); j++){
			// 取出每个元素的n位
			int digitOfElement = arr[j]/n % 10;
			// 放入到对应的桶中
			bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[j];
			bucketElementCounts[digitOfElement]++;
		} 
		int index = 0;
		// 遍历每一个桶，并将桶中的数据放入到元素中
		for(int k = 0; k < bucket.size(); k++){
			// 如果桶中有数据，才放入原数组
			if(bucketElementCounts[k] != 0){
				// 循环该桶 放入原数组 
				for(int l = 0; l < bucketElementCounts[k]; l++){
					// 取出元素放入到arr中
					arr[index] = bucket[k][l];
					index++; 
				} 
			}
			// 第i+1轮结束后，需要将每个bucketElementCounts[k] = 0
			bucketElementCounts[k] = 0;
		}
		cout << "第"<< i+1 <<"轮排序结果:"<< endl;
		cout << arr << endl;  
		index = 0;		
	}

}

int main(){
	
	vector<int> array{53,3,542,748,14,214};
	cout << "基数排序前:" <<endl;
	cout << array <<endl;
	RadixSort(array);
	cout << "基数排序后: " << endl; 
	cout << array << endl;
	system("pause");
	
	return 0;
}  
// 有负数就不要使用基数排序
```

![](D:\CandCpp\note\数据结构与算法\img\排序\排序算法对比.PNG)
