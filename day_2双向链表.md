## 1.双向链表  

```c++
#include<iostream>
#include<string>
using namespace std;

// 实现双向链表

class HeroNode {
	public:
		int id;
		string name;
		string nickname;
		HeroNode* next = NULL;
		HeroNode* pre = NULL;
	// 构造函数
		HeroNode(int id, string name, string nickname) {
			this->id = id;
			this->name = name;
			this->nickname = name;
		}
	// 析构函数
		//~HeroNode(){

		//}
};

class LinkedList {
	// 创建头结点
	private:
		HeroNode* head = new HeroNode(0, "", "");
	public:
		// 构造函数
		// 插入节点
		void add(HeroNode *heroNode) {
			// 找到最后一个节点
			HeroNode* temp = head;
			while (temp->next != NULL) {
				temp = temp->next;
			}
			temp->next = heroNode;
			heroNode->pre = temp;
			cout << "添加成功!"<<endl;

			//// 释放temp
			//if (temp != NULL) {
			//	delete temp;
			//	temp = NULL;
			//}
		}
		// 删除节点
		void deleteNode(int No) {
			if (head->next == NULL) {
				cout << "双向链表为空！" << endl;
				return;
			}
			// 从头结点开始遍历
			HeroNode* temp = head->next;
			while (temp != NULL) {
				if (temp->id == No) {
					if (temp->next == NULL) {
						temp->pre->next = NULL;
						delete temp;
						temp = NULL;
						cout << "节点删除成功！" << endl;
						return;
					}
					else {
						// 找到id==No的节点删除
						temp->next->pre = temp->pre;
						temp->pre->next = temp->next;
						// 节点删除成功
						delete temp;
						temp = NULL;
						cout << "节点删除成功！" << endl;
						return;
					}
				}
				temp = temp->next;
			}
			cout << "未找到节点！" << endl;
		}
		// 前向遍历双向链表
		// 后向遍历双向链表
		// 析构函数
		~LinkedList() {
			if (head != NULL) {
				delete head;
				head = NULL;
				cout << "头结点成功释放！" << endl;
			}
		}
};

int main() {
	LinkedList list;
	HeroNode* hero1 = new HeroNode(1, "宋江", "及时雨");
	HeroNode* hero2 = new HeroNode(2, "卢俊义", "玉麒麟");
	HeroNode* hero3 = new HeroNode(3, "吴用", "智多星");
	HeroNode* hero4 = new HeroNode(4, "林冲", "豹子头");
	list.add(hero1);
	list.add(hero2);
	list.add(hero3);
	list.add(hero4);
	int No;
	cout << "输入您要删除的编号" << endl;
	cin >> No;
	list.deleteNode(No);

	system("pause");
	return 0;
	
}
```