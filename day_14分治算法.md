### 分治算法

```c++
// 分治算法
/*
	分而治之，把一个复杂的问题分成两个或多个相似的子问题。
	二分搜查、大整数乘法、棋盘覆盖、合并排序、快速排序、线性时间选择、汉诺塔 
*/
/*
	分治算法的基本步骤
	1.分解:将原问题分解为若干个规模较小，相互独立，与原问题形式相同的子问题。 
	2.解决:若子问题规模较小而容易被解决则直接解，否则递归地解各个子问题。 
	3.合并:将各个子问题的解合并为原问题的解。 
*/ 

/*
	汉诺塔问题思路分析:
	1.如果只有一个盘 A->C
	2.如果有n >= 2情况，总是可以看做是两个盘 1.最下边的盘 2.上边的盘
		1.先把最上面的盘 A->B
		2.把最下面的盘 A->C
		3.把B塔的所有盘从 B->C 

*/ 
# include <iostream>
# include <string>
# include <vector>
using namespace std;

// 汉诺塔移动方法
// 使用分治算法解决
void hanoiTower(int num, char a, char b, char c){
	// 如果只有一个盘
	if(num==1){
		cout << "第1个盘从 "<< a << " -> " << c << endl;
	}else{
		// 1.把最上面的盘A->B 移动过程中使用到c 
		hanoiTower(num-1, a, c, b); 
		// 2.把最下边的盘A->C
		cout << "第" << num << "个盘从 "<< a << " -> " << c << endl; 
		// 3.把B塔的所有盘移动到C 移动过程中使用到a
		hanoiTower(num-1, b, a, c); 
	}
} 

int main(){
	
	
	hanoiTower(5, 'A', 'B', 'C');
	
	system("pause");
	return 0;
} 
```

