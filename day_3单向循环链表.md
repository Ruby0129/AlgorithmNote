### 1.单向循环链表

```c++
// 解决约瑟夫问题
#include<iostream>
#include<string>
using namespace std;

// 使用单向循环链表解决约瑟夫环问题

// Boy类表示一个节点
class Boy {
private:
	int no; // 编号
	Boy* next = NULL; // 指向下一个节点 
public:
	Boy(int no) {
		this->no = no;
	}
	int getNo() {
		return this->no;
	}
	void setNo(int no) {
		this->no = no;
	}
	Boy *getNext() {
		return this->next;
	}
	void setNext(Boy* boy) {
		this->next = boy;
	}
};

// 创建一个环形的单向链表
class CircleSingleLinkedList {
private:
// 创建一个first节点,当前没有编号
	Boy* first = new Boy(-1);

public:
	// 添加节点
	void addBoy(int nums) {
		// nums为添加的节点数
		// 做数据校验
		if (nums < 1) {
			cout<<"nums值不正确!"<<endl;
			return;
		}
		// 创建循环链表
		// 辅助指针帮助构建环形链表
		Boy* curBoy = NULL;
		for (int i = 1; i <= nums; i++) {
			// 根据编号创建节点
			Boy *boy = new Boy(i);
			// 如果是第一个节点
			if (i == 1) {
				first = boy;
				first->setNext(first); // 构成环
				curBoy = first;
			}
			curBoy->setNext(boy);
			boy->setNext(first);
			curBoy = boy;
		}
		// 释放堆区空间
	}
	// 遍历当前环形链表所有的节点
	void showBoy() {
		// 判断链表是否为空
		if (first == NULL) {
			cout << "链表为空" << endl;
			return;
		}
		// 使用辅助指针完成遍历
		Boy* curBoy = first;
		int nums = 0;
		while (true) {
			cout << "编号: " << curBoy->getNo() << " ";
			nums++;
			if (curBoy->getNext() == first) {
				cout << endl;
				// 已经遍历完毕
				cout << "遍历完毕,共有" << nums << "个节点" << endl;
				break;
			}
			curBoy = curBoy->getNext();
		}
	}
	// 根据用户的输入，计算出出圈的顺序
	// startNo表示从第几个开始数
	// countNum表示数几下
	// nums表示最初有多少个在圈中
	void countBoy(int startNo, int countNum, int nums) {
		// 数据校验
		if (first == NULL || startNo<1 || startNo>nums) {
			cout << "参数输入有误，请重新输入!" << endl;
			return;
		}
		// 创建辅助指针，帮助完成出圈
		Boy* helper = first;
		while (true) {
			// 先让helper指向最后一个节点
			if (helper->getNext() == first) {
				// 说明helper指向了最后一个节点
				break;
			}
			helper = helper->getNext();
		}
		// 包数前先让first和helper移动k-1次
		for (int i = 0; i < startNo - 1; i++) {
			first = first->getNext();
			helper = helper->getNext();
		}
		// 然后出圈
		// 一个循环操作，知道圈中只有一个节点
		while (true) {
			if (helper == first) {
				break;
			}
			// 让first和helper同时移动countNum-1,然后出圈
			for (int j = 0; j < countNum - 1; j++) {
				first = first->getNext();
				helper = helper->getNext();
			}
			// 这时first指向的节点，就是要出圈的节点
			cout << "编号:" << first->getNo() << "出圈" << endl;
			Boy* temp = first;
			first = first->getNext();
			helper->setNext(first);
			// 释放堆区内存
			if (temp != NULL) {
				delete temp;
				temp = NULL;
				//cout << "节点以释放" << endl;
			}
		}
		cout << "最后留在圈中的节点编号:" << first->getNo() << endl;
	}
};

int main() {
	CircleSingleLinkedList list;
	list.addBoy(5);
	list.showBoy();
	// 测试出圈是否正确
	list.countBoy(1, 2, 5);
	system("pause");
	return 0;
}
```

