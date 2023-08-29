### 递归

```c++
#include<iostream>
using namespace std;

// 递归
/*
	递归调用规则
	1.当程序执行到一个方法时，就会开辟一个独立的空间(栈)。 
	2.每个空间的数据(局部变量),是独立的。 
*/
// 打印数字 
void test(int n){
	if(n > 2){
		test(n-1);
	}
	cout<<"n = "<<n<<endl;
}

// 阶乘
int factorial(int n){
	if(n == 1){
		return 1;
	}else{
		return n*factorial(n - 1);
	}
}  

int main(){
	
	test(100);
	cout<<factorial(5)<<endl;
	system("pause");
	return 0;
} 

// 递归用于解决什么样的问题
/*
1.各种数学问题:8皇后 汉诺塔 阶乘 迷宫 球和篮子的问题
2.快排、归并排序、二分查找、分治算法
3.将用栈解决的问题->递归代码比较简洁
*/
```

### 迷宫问题

```c++
#include<iostream>
#include<vector>
using namespace std; 
// 递归解决迷宫问题 
// 使用递归回溯给小球找路

/*
	说明 
	1.map地图
	2.i j 从哪个位置开始找 (1, 1)   
	3.如果小球能到map(6, 5)位置，则说明通路找到 
	 找到通路返回true 否则返回false 
	4.约定:当map[i][j] 为 0 表示该点没有走过 为1表示墙; 2表示通路可以走; 3表示该点已经走过，但是走不通 
	5.在走迷宫时，要确定一个策略,下-->右-->上-->左 如果该点走不通,回溯到上一步重新走 
*/
bool setWay(int map[][7], int i, int j){
	if(map[6][5] == 2){
		// 通路已经找到
		return true; 
	}else{
		if(map[i][j] == 0){
			// 如果当前这个点没有走过
			// 按照策略走
			map[i][j] = 2; //假定该点可以走通
			if(setWay(map, i+1, j)) {
				// 向下走
				 return true; 
			}else if(setWay(map, i, j+1)){
				// 向右走
				return true; 
			}else if(setWay(map, i-1, j)){
				// 向上走
				return true; 
			}else if(setWay(map, i, j-1)){
				// 向左走 
				return true;
			}else{
				map[i][j] = 3;
				return false;
			}
		}else{
			// 如果map[i][j]!=0,可能是1,2,3
			return false; 
		}
	}
} 

int main(){
	
	// 先创建一个二维数组，模拟迷宫
	// 地图 
	//vector<vector<int>> map(8, vector<int>(7));
	int map[8][7] = {0};
	// 使用1表示墙
	// 上下全部置为1
	for(int i=0; i<7; i++){
		map[0][i] = 1;
		map[7][i] = 1;
	} 
	// 左右全部置为1
	for(int i=0; i<8; i++){
		map[i][0] = 1;
		map[i][6] = 1;
	}
	// 设置挡板 1表示
	map[3][1] = 1;
	map[3][2] = 1; 
	// 使用递归回溯给小球找路
	setWay(map, 1, 1); 
	// 输出地图
	cout<<"地图的情况"<<endl;
	for(int i=0;i<8;i++){
		for(int j=0;j<7;j++){
			cout<<map[i][j]<<" ";
		}
		cout<<endl;
	} 
	system("pause");
	return 0;
}
```

### 八皇后问题

```c++
#include<iostream>
#include<vector>
#include<stdlib.h>
using namespace std; 
// 八皇后问题 
/*
思路分析
1.把第一个皇后放在第一行第一列
2.第二个皇后放在第二行第一列，然后判断是否OK，如果不OK，继续放在第二列、第三列、依次把所有列都放完，找到一个合适的位置
3.继续第三个皇后，还是第一列、第二列。。。直到第八个皇后也能放在一个不冲突的位置，算是得到了一个正确解
4.当得到一个正确解后，再退回到上一个栈时，就会开始回溯，即将第一个皇后放在第一列的所有正确解全部得到
5.然后回头继续第一个皇后放第二列，后面继续循环执行1、2、3、4的步骤 
*/
// 写一个方法 

// 定义一个max表示共有多少个皇后
#define max 8

static int count = 0;
int array[max];
// 查看当我们放置第n个皇后，就去检测该皇后是否和前面已经摆放的皇后冲突
// n表示第n个皇后 
bool judge(int n){
	for(int i = 0; i < n; i++){
		// 说明
		// 1.array[i] == array[n] 第n个皇后是否和前面的n-1个皇后在同一列 
		// 2.abs(n-i) == abs(*(array+n) - *(array - i)) 表示判断第n个皇后是否和第i个皇后在同一斜线
		// 3.判断是否在同一行 
		if(*(array+i) == *(array+n) || abs(n-i) == abs(*(array+n) - *(array+i)) ){
			return false;
		}
	}
	return true;
} 

// 将皇后摆放的位置输出
void printQueen(){
	count++;
	for(int i=0;i<max;i++){
		cout<<*(array+i)<<" ";
	}
	cout<<endl;
} 

// 编写方法，放置第n个皇后
// 特别注意:check是每一次递归时，进入到check中，都有一次for循环 因此会有回溯 
void check(int n){
	if(n == max){
		// n = 8
		// 8个皇后已经放好
		printQueen(); 
		return;
	}
	// 依次放入皇后，并判断是否冲突
	for(int i=0;i<max;i++){
		// 先把当前这个皇后n，放到该行的第一列
		array[n] = i;
		// 判断当放置第n个皇后到i列时，是否冲突
		if(judge(n)){
			// 不冲突，接着放n+1个皇后，开始递归
			check(n+1); 			 
		}
		// 如果冲突，就继续执行array[n] = i，即将第n个皇后，放置在本行后移的一个位置。 		 
	} 
} 


int main(){
	// 定义数组array,保存皇后放置位置的结果比如 arr = {0, 4, 7, 5, 2, 6, 1, 3}
	//sint array[max];
	// 测试八皇后是否正确
	check(0); 
	cout<<"共"<<count<<"种摆法"<<endl;
	system("pause");
	return 0;
}
```

