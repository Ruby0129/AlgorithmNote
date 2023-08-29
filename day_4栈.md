

### 1.数组模拟栈

```c++
#include<iostream>
#include<string>
using namespace std;

// 数组模拟栈
/*
思路分析
1.使用数组来模拟栈
2.定义一个top来表示栈顶,初始化为-1
3.入栈的操作,当有数据加入到栈时，top++; stack[top] = data;
4.出栈的操作,int value = stack[top]; top--; return value
*/

// 栈结构
class ArrayStack {

private:
	int maxSize; //栈的大小
	int* stack = NULL; //数组
	int top = -1; // 栈顶 初始-1
public:
	// 构造函数
	ArrayStack(int maxSize) {
		this->maxSize = maxSize;
		stack = new int[this->maxSize];		
		cout << "stack初始化成功!" << endl;
	}

	// 栈满
	bool isFull() {
		return this->top == maxSize - 1;
	}

	// 栈空
	bool isEmpty() {
		return top == -1;
	}

	// 入栈
	void push(int value) {
		// 判断栈是否满
		if (this->isFull()) {
			cout << "栈已满" << endl;
			return;
		}
		top++;
		*(stack + top) = value;
	}

	// 出栈 将栈顶的数据返回
	void pop(int &value) {
		// 判断栈是否空
		if (this->isEmpty()) {
			cout << "栈为空" << endl;
			return;
		}
		value = *(stack + top);
		top--;
	}
	// 遍历栈
	void list() {
		// 遍历时，需要从栈顶开始显示数据
		if (this->isEmpty()) {
			cout << "没有数据,栈为空!" << endl;
			return;
		}
		for (int i = top; i >= 0; i--) {
			cout << *(this->stack + i) << " ";
		}
		cout << endl;
	}
	// 析构函数
	~ArrayStack() {
		// 释放堆区申请的内存
		if (this->stack != NULL) {
			// 释放stack数组
			delete[]stack;
			stack = NULL;
		}
		cout << "stack释放成功!" << endl;
	}
};

int main() {

	// 测试ArrayStack
	ArrayStack stack(4);
	int key = 0;
	bool loop = true;
	while (loop) {
		cout << "1:显示栈" << endl;
		cout << "2:添加数据到栈" << endl;
		cout << "3:从栈取出数据" << endl;
		cout << "其他:退出程序" << endl;
		cout << "请输入你的选择:" << endl;
		cin >> key;
		switch (key) {
		case 1:
			stack.list();
			break;
		case 2:
			cout << "请输入一个数字:" << endl;
			int value;
			cin >> value;
			stack.push(value);
			break;
		case 3:
			{
				int res = 0;
				stack.pop(res);
				cout << "出栈的数据为:" << res << endl;
				break; 
			}
		default:
			loop = false;
			cout << "程序已退出" << endl;
			break;
		}
		system("pause");
		system("cls");
	}
	system("pause");
	return 0;
}
```

### 2.栈实现综合计算器

```c++
#include<iostream>
#include<string>
using namespace std;

// 使用栈完成计算一个表达式的结果
// 表达式无括号 假定只有 + - * /
// 两个栈 数栈 符号栈
/*
1.通过一个index值(索引),来遍历我们的表达式
2.如果发现是一个数字,就直接入数栈
3.如果发现扫描到的是一个符号,就分如下情况来解决
	3.1 如果当前的符号栈为空，就直接入栈
	3.2 如果符号栈内有操作符，就进行比较，如果当前的操作符的优先级小于或者等于栈中的操作符，
		就需要从数栈中pop出两个数，然后再从符号栈中pop出一个符号，进行运算，将得到的运算结果入数栈，
		然后将当前的操作符入符号栈。如果当前操作符的优先级大于栈中的操作符，就将当前操作符直接入栈,不计算。
4.当扫描完毕过后，就顺序的从数栈和符号栈中pop出相应的数和符号并运算。
5.最后在数栈中只会有一个数字，就是表达式的结果。
*/

// 先创建一个栈
// 栈结构
template <typename T>
class ArrayStack {

private:
	int maxSize; //栈的大小
	T* stack = NULL; //数组
	int top = -1; // 栈顶 初始-1
public:
	// 构造函数
	ArrayStack(int maxSize) {
		this->maxSize = maxSize;
		stack = new T[this->maxSize];		
		cout << "stack初始化成功!" << endl;
	}
	// 返回栈顶元素
	int peek() {
		if (this->isEmpty()) {
			cout << "栈为空" << endl;
			return -1;
		}
		return this->stack[top];
	}
	// 栈满
	bool isFull() {
		return this->top == maxSize - 1;
	}

	// 栈空
	bool isEmpty() {
		return top == -1;
	}

	// 入栈
	void push(T value) {
		// 判断栈是否满
		if (this->isFull()) {
			cout << "栈已满" << endl;
			return;
		}
		top++;
		*(stack + top) = value;
	}

	// 出栈 将栈顶的数据返回
	void pop(T &value) {
		// 判断栈是否空
		if (this->isEmpty()) {
			cout << "栈为空" << endl;
			return;
		}
		value = *(stack + top);
		top--;
	}
	// 遍历栈
	void list() {
		// 遍历时，需要从栈顶开始显示数据
		if (this->isEmpty()) {
			cout << "没有数据,栈为空!" << endl;
			return;
		}
		for (int i = top; i >= 0; i--) {
			cout << *(this->stack + i) << " ";
		}
		cout << endl;
	}
	// 返回运算符的优先级，优先级由程序员来确定,优先级使用数字表示
	// 数字越大则优先级越高
	int priority(char oper) {
		if (oper == '*' || oper == '/') {
			return 1;
		}
		else if (oper == '+' || oper == '-') {
			return 0;
		}
		else {
			cout << "操作符错误!" << endl;
			return -1;
		}
	}
	// 判断是不是一个运算符
	bool isOper(char val) {
		return val == '+' || val == '-' || val == '*' || val == '/';
	}
	// 计算方法
	int cal(int num1, int num2, char oper) {
		int res = 0; // res用于存放计算的结果
		switch (oper) {
			case '+':
				res = num1 + num2;
				break;
			case '-':
				res = num2 - num1;
				break;
			case '*':
				res = num1 * num2;
				break;
			case '/':
				res = num2 / num1;
				break;
			default:
				break;
		}
		return res;
	}
	// 析构函数
	~ArrayStack() {
		// 释放堆区申请的内存
		if (this->stack != NULL) {
			// 释放stack数组
			delete[]stack;
			stack = NULL;
		}
		cout << "stack释放成功!" << endl;
	}
};

int main() {
	// 完成表达式的运算
	string expression = "3+2*6-2";
	// 创建两个栈,数栈和符号栈
	ArrayStack<int>* numStack = new ArrayStack<int>(10);
	ArrayStack<char>* operStack = new ArrayStack<char>(10);
	// 定义需要的相关变量
	int index = 0; //用于扫描
	int num1 = 0;
	int num2 = 0;
	char oper = ' ';
	int res = 0;
	char ch = ' '; // 将每次扫描的char保存到ch
	// while循环扫描expression
	while (true) {
		// 先依次得到expression的每一个字符
		ch = expression.substr(index, 1)[0];
		// 判断ch是什么，然后做响应的处理
		if (operStack->isOper(ch)) {
			// 如果是运算符
			// 判断当前符号栈是否为空
			if (!operStack->isEmpty()) {
				// 不为空

				// 如果当前的操作符的优先级小于或者等于栈中的操作符
				if (operStack->priority(ch) <= operStack->priority(operStack->peek())) {
					// 从数栈中pop出两个数，然后再从符号栈中pop出一个符号，进行运算
					numStack->pop(num1);
					numStack->pop(num2);
					operStack->pop(oper);
					res = numStack->cal(num1, num2, oper);
					// 把结果入数栈
					numStack->push(res);
					// 把当前操作符入符号栈
					operStack->push(ch);
				}
				else {
					// 如果当前的操作符的优先级大于栈中的操作符
					operStack->push(ch);
				}
			}
			else {
				// 为空，直接入栈
				operStack->push(ch);
			}
		}
		else {
			// 如果是数，则直接入数栈
			numStack->push(atoi(&ch)); 
		}
		// 让index+1,并判断是否扫描到expression的最后
		index++;
		if (index >= expression.size()) {
			break;
		}	
	}
	//当扫描完毕过后，就顺序的从数栈和符号栈中pop出相应的数和符号并运算。
	while (true) {
		// 如果符号栈为空，则计算到最后的结果，数栈中只有一个数字
		if (operStack->isEmpty()) {
			break;
		}
		numStack->pop(num1);
		numStack->pop(num2);
		operStack->pop(oper);
		res = numStack->cal(num1, num2, oper);
		// 把结果入数栈
		numStack->push(res);
	}
	int result = 0;
	numStack->pop(result);
	cout << "表达式" << expression << "的结果是" << result << endl;
	system("pause");
	return 0;
}
```

```java
// 前缀表达式(波兰表达式)
// 1.前缀表达式又称波兰式，前缀表达式的运算符位于操作数之前
// 例如 (3+4)X5-6对应的前缀表达式就是-X+3456
// 前缀表达式的计算机求值
/*
	从右至左扫描表达式，遇到数字时，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数，用运算符对它们做相应的运算(栈顶元素和次顶元素),
	并将结果入栈；重复上述过程到表达式最左端，最后运算得出的值即为表达式的结果。  减法是先弹出来的数减去后弹出来的数 
*/ 

// 中缀表达式
/*
	最常见的运算表达式
	对人熟悉，对计算机不好操作，因此在计算结果时，往往会将中缀表达式转成其他表达式来操作(一般转化成后缀表达式) 
*/ 

// 后缀表达式(逆波兰表达式) 
/*
	与前缀表达式相似，只是运算符位于操作数之后
	例如(3+4)x5-6对应的后缀表达式就是 3 4 + 5 x 6 - 
	后缀表达式的计算机求值
	从左至右扫描表达式，遇到数字时，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数，用运算符对它们做相应的运算(次顶元素和栈顶元素)，并将结果入栈；
	重复上述过程直到表达式最右端，最后运算得出的值即为表达式的结果。  减法是后弹出来的数减去先弹出来的数
*/ 
```

```java
// 逆波兰表达式实现
package stack;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class PolandNotation {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// 逆波兰表达式实现
		// 先定义一个逆波兰表达式
		// 1.为了方便，逆波兰表达式的数字和符号使用空格隔开
		String suffixExpression = "3 4 + 5 * 6 - ";
		// 思路
		// 1. 先将suffixExpression放到ArrayList中
		List<String> rpnList = getListString(suffixExpression);
		System.out.println("rpnList = " + rpnList);
		// 2. 将ArrayList传递给一个方法，遍历ArrayList配合栈完成计算
		int res = calculate(rpnList);
		System.out.println("result = " + res);

	}
	// 将一个逆波兰表达式，依次将数据和运算符放入到ArrayList中
	public static List<String> getListString(String suffixExpression){
		// 将suffixExpression分割
		String[] split = suffixExpression.split(" ");
		List<String> list = new ArrayList<String>();
		for(String ele:split){
			list.add(ele);
		}
		return list;
	}
	
	// 完成对逆波兰表达式的运算
	/*
	 * 1.从左至右扫描，将3和4压入栈中
	 * 2.遇到+运算符，因此弹出4和3，计算出4+3的值，得7，再将7入栈
	 * 3.将5入栈
	 * 4.接下来是x运算符，因此弹出5和7，计算出7x5=35,将35入栈
	 * 5.将6入栈
	 * 6.最后是-运算符，计算出35-6的值，即29得出最后结果
	 * */
	
	public static int calculate(List<String> ls){
		// 创建栈，只需要一个栈即可
		Stack<String> stack= new Stack<String>();
		// 遍历ls
		for(String item:ls){
			// 这里使用正则表达式来取出数
			if(item.matches("\\d+")){
				// 匹配的是多位数
				//入栈
				stack.push(item);
			}else{
				// pop出两个数并运算，再入栈
				int num2 = Integer.parseInt(stack.pop());
				int num1 = Integer.parseInt(stack.pop());
				int res = 0;
				if(item.equals("+")){
					res = num1 + num2;
				}else if(item.equals("-")){
					res = num1 - num2;
				}else if(item.equals("*")){
					res = num1 * num2;
				}else if(item.equals("/")){
					res = num1 / num2;
				}else{
					throw new RuntimeException("运算符有误！");
				}
				stack.push(""+res);
			}
		}
		// 最后留在stack中的就是运算结果
		return Integer.parseInt(stack.pop());
	}
}
```

### 中缀转后缀表达式

```java
package stack;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class PolandNotation {

	public static void main(String[] args) {
		
		// 完成一个将中缀表达式转成后缀表达式的功能
		// 说明
		// 1. 1+((2+3)x4)-5=>转成1 2 3 + 4 x + 5 -
		// 2. 直接对字符串进行操作不方便，因此先将字符串转成中缀表达式对应的List
		// 即1+((2+3)x4)-5 => ArrayList[1, +, (, (, 2, +, 3, ), x, 4, ), -, 5]
		String expression = "1+((2+3)*4)-5";
		List<String> infixExpressionList = toInfixExpressionList(expression);
		System.out.println(infixExpressionList);
		// 3.将得到的中缀表达式的List 转成 后缀表达式对应的List
		// 即ArrayList[1, +, (, (, 2, +, 3, ), x, 4, ), -, 5] => ArrayList[1 2 3 + 4 x + 5 -]
		List<String> SuffixExpressionList = parseSuffixExpressionList(infixExpressionList);
		System.out.println("后缀表达式对应的List"+SuffixExpressionList);
		
		System.out.printf("expression = %d", calculate(SuffixExpressionList));
		
//		// TODO Auto-generated method stub
//		// 逆波兰表达式实现
//		// 先定义一个逆波兰表达式
//		// 1.为了方便，逆波兰表达式的数字和符号使用空格隔开
//		String suffixExpression = "30 4 + 5 * 6 - ";
//		// 思路
//		// 1. 先将suffixExpression放到ArrayList中
//		List<String> rpnList = getListString(suffixExpression);
//		System.out.println("rpnList = " + rpnList);
//		// 2. 将ArrayList传递给一个方法，遍历ArrayList配合栈完成计算
//		int res = calculate(rpnList);
//		System.out.println("result = " + res);

	}
	// 将一个逆波兰表达式，依次将数据和运算符放入到ArrayList中
	public static List<String> getListString(String suffixExpression){
		// 将suffixExpression分割
		String[] split = suffixExpression.split(" ");
		List<String> list = new ArrayList<String>();
		for(String ele:split){
			list.add(ele);
		}
		return list;
	}
	
	// 完成对逆波兰表达式的运算
	/*
	 * 1.从左至右扫描，将3和4压入栈中
	 * 2.遇到+运算符，因此弹出4和3，计算出4+3的值，得7，再将7入栈
	 * 3.将5入栈
	 * 4.接下来是x运算符，因此弹出5和7，计算出7x5=35,将35入栈
	 * 5.将6入栈
	 * 6.最后是-运算符，计算出35-6的值，即29得出最后结果
	 * */
	
	public static int calculate(List<String> ls){
		// 创建栈，只需要一个栈即可
		Stack<String> stack= new Stack<String>();
		// 遍历ls
		for(String item:ls){
			// 这里使用正则表达式来取出数
			if(item.matches("\\d+")){
				// 匹配的是多位数
				//入栈
				stack.push(item);
			}else{
				// pop出两个数并运算，再入栈
				int num2 = Integer.parseInt(stack.pop());
				int num1 = Integer.parseInt(stack.pop());
				int res = 0;
				if(item.equals("+")){
					res = num1 + num2;
				}else if(item.equals("-")){
					res = num1 - num2;
				}else if(item.equals("*")){
					res = num1 * num2;
				}else if(item.equals("/")){
					res = num1 / num2;
				}else{
					throw new RuntimeException("运算符有误！");
				}
				stack.push(""+res);
			}
		}
		// 最后留在stack中的就是运算结果
		return Integer.parseInt(stack.pop());
	}
	// 讲中缀表达式转成对应的List
	public static List<String> toInfixExpressionList(String s){
		// 定义一个List, 存放中缀表达式对应的内容
		List<String> ls = new ArrayList<String>();
		int i = 0; // 用于遍历中缀表达式字符串
		String str; // 对多位数的拼接
		char c; // 每遍历到一个字符就放入到c
		do{
			// 如果c是一个非数字，就需要加入到ls中
			if((c=s.charAt(i))<48 || (c=s.charAt(i))>57){
				// 不是一个数字
				ls.add(""+c);
				i++;
			}else{
				// 如果是数字就需要考虑多位数
				str = ""; //先将str置成空串
				while(i<s.length()&&(c=s.charAt(i))>=48&&(c=s.charAt(i))<=57){
					str += c;//拼接
					i++;
				}
				ls.add(str);
			}
		}while(i<s.length());
		return ls;
	}
	
	//将得到的中缀表达式的List 转成 后缀表达式对应的List
	public static List<String> parseSuffixExpressionList(List<String> ls){
		
		// 定义两个栈
		Stack<String> s1 = new Stack<String>(); //符号栈
		//储存中间结果的栈 可以用ArrayList替代（因为整个过程没有pop操作）
		List<String> s2 = new ArrayList<String>();
		// 遍历ls
		for(String item:ls){
			// 如果是一个数 加入到s2
			if(item.matches("\\d+")){
				s2.add(item);
			}else if(item.equals("(")){
				s1.push(item);
			}else if(item.equals(")")){
				//如果是右括号")", 则依次弹出s1栈顶的运算符，并压入s2，直到遇到左括号为止，此时将这一对括号丢弃
				while(!(s1.peek().equals("("))){
					s2.add(s1.pop());
				}
				s1.pop(); //将(弹出s1栈
			}else{
				// 当item的优先级小于等于s1栈顶的运算符的优先级
				// 将s1栈顶的运算符弹出并压入到s2中，再次与s1中新的栈顶运算符相比较；
				// 问题:缺少一个比较运算符优先级的方法
				while(s1.size()!=0&&Operation.getValue(s1.peek())>=Operation.getValue(item)){
					s2.add(s1.pop());
				}
				// 将当前扫描的运算符压入s1
				s1.push(item);
			}
		}
		// 将s1中剩余的运算符依次弹出并加入s2
		while(s1.size()!=0){
			s2.add(s1.pop());
		}
		return s2; // 因为是存放再List中，因此按顺序输出就是对应的后缀表达式对应的List
	} 
}

// 编写一个类 Operation 可以返回一个运算符对应的优先级
class Operation{
	private static int ADD = 1;
	private static int SUB = 1;
	private static int MUL = 2;
	private static int DIV = 2;
	
	// 写一个方法，返回对应的优先级数字
	public static int getValue(String operation){
		int result = 0;
		switch(operation){
			case  "+":
				result = ADD;
				break;
			case  "-":
				result = SUB;
				break;
			case  "*":
				result = MUL;
				break;
			case  "/":
				result = DIV;
				break;
			default:
				System.out.println("不存在该操作符");
				break;
		}
		return result;
	}
}


```

