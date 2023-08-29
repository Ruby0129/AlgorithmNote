## 1.链表 ##

```c++
class HeroNode{
	public:
		int no;
		string name;
		string nickname;
		HeroNode* next; //指向下一个节点
	
		// 无参构造
		HeroNode() {
	
		}
		// 构造函数
		HeroNode(int hNo, string hName, string hNickname) {
			this->no = hNo;
			this->name = hName;
			this->nickname = hNickname;
		}
		// 析构函数
		~HeroNode() {
	
		}
};

class LinkedList {
	// 初始化头结点， 头结点不要动
	private:
		HeroNode* head = new HeroNode(0, " ", " ");
	
	public:
		// 添加节点
		void add(HeroNode *heroNode) {
			// 当不考虑编号顺序时
			// 1、找到当前链表的最后一个节点
			// 2、将最后一个节点的next域指向新的节点
			// head节点不能动，需要辅助指针
			HeroNode* temp = head;
			// 遍历链表找到最后
			while (temp->next != NULL) {
				temp = temp->next;
			}
			temp->next = heroNode;
		}
		// 按编号添加
		void addByOrder(HeroNode* heroNode) {
			// 1、首先找到新添加的节点位置
			// 2、新的节点的next = temp->next
			// 3、temp->next = 新的节点
			HeroNode* temp = head;
			bool flag = false; // 标志添加的编号是否存在，默认为false
			// temp找到添加位置的前一个节点
			while (true) {
				if (temp->next == NULL) {
					// temp已经在链表的最后
					temp->next = heroNode;
					heroNode->next = NULL;
					break;
				}
				else if (temp->next->no > heroNode->no) {
					// 位置找到了，就在temp的后面插入
					heroNode->next = temp->next;
					temp->next = heroNode;
					break;
				}
				else if (temp->next->no == heroNode->no) {
					// 希望添加的编号已经存在
					cout << "不能添加，编号已存在" << endl;
					break;
				}
				temp = temp->next;
			}
		}
		// 修改节点的信息，根据编号No来修改. No不能修改
		void update(int No, string new_Name, string new_NickName) {
			if (head->next == NULL) {
				cout << "链表为空" << endl;
				return;
			}
			// 找到需要修改的节点,根据No编号
			HeroNode* temp = head->next;
			bool flag = false;
			while (temp != NULL) {
				if (temp->no == No) {
					temp->name = new_Name;
					temp->nickname = new_NickName;
					break;
				}
				if (temp->next == NULL) {
					cout << "未找到该编号的英雄" << endl;
					break;
				}
				temp = temp->next;
			}
			
		}
		// 删除节点
		void deleteNode(int No) {
			// 1、找到待删除结点的前一个结点temp
			// 2、temp_Node = temp->next;
			//  temp->next = temp->next->next;
			// delete temp_Node;
			// temp_Node = null;
			if (head == NULL) {
				cout << "链表为空" << endl;
				return;
			}
			HeroNode *temp = head;
			while (temp->next != NULL) {
				if (temp->next->no == No) {
					HeroNode* temp_Node = temp->next;
					temp->next = temp->next->next;
					if (temp_Node != NULL) {
						delete temp_Node;
						temp_Node = NULL;
						cout << "结点释放成功！" << endl;
					}
					break;
				}
			}
		}
		// 获取到单链表的节点的个数(如果带头结点的链表，不统计头结点)
		int getLength() {
			int len = 0;
			if (head->next == NULL) {
				return 0;
			}
			HeroNode* temp = head->next;
			while (temp != NULL) {
				len++;
				temp = temp->next;
			}
			return len;
		}
	
		// 显示链表
		void list() {
			// 通过辅助变量遍历链表
			// 1、先判断链表是否为空
			if (head->next == NULL) {
				cout << "链表为空" << endl;
				return;
			}
			HeroNode* temp = head->next;
			while (temp!= NULL) {
				cout << "排名:" << temp->no << " "
					<< "昵称:" << temp->nickname << " "
					<< "姓名:" << temp->name << " "
					<< endl;
				temp = temp->next;
			}
		}
		// 将单链表进行反转
		void reverseList() {
			if (head->next == NULL || head->next->next == NULL) {
				// 如果当前链表为空或者只有一个节点就无需反转
				return;
			}
	
			// 定义一个辅助的指针，帮助遍历原来的链表
			HeroNode* cur = head->next;
			HeroNode* next = NULL; //指向当前节点的下一个节点
			HeroNode* reverseHead = new HeroNode(0, "", "");
			// 遍历原来的链表
			// 没遍历一个节点，就将其取出，放在新的链表reverseHead的最前端
			while (cur != NULL) {
				next = cur->next;
				cur->next = reverseHead->next;
				reverseHead->next = cur;
				cur = next;// 让cur后移
			}
			// 将head->next 指向 reverseHead->next
			head->next = reverseHead->next;
			if (reverseHead != NULL) {
				delete reverseHead;
				reverseHead = NULL;
				cout << "reverseHead释放完毕" << endl;
			}
		}
		// 逆序打印单链表
		void reversePrint() {
			if (head->next == NULL) {
				return; //空链表
			}
			// 创建栈将各个节点压入栈中
			stack<HeroNode*> Stack;
			HeroNode* cur = head->next;
			/*
				可以利用栈，将各个节点压入栈中，利用栈的先进后出特点
			*/
			// 将链表的所有节点压入栈中
			while (cur!= NULL) {
				Stack.push(cur);
				cur = cur->next;
			}
			// 将栈中的节点进行打印
			while (!Stack.empty()) {
				cout << Stack.top()->no << " " << Stack.top()->nickname << " " << Stack.top()->name << endl;
				Stack.pop();
			}
		}
		// 析构函数
		~LinkedList(){
			if (head != NULL) {
			delete head;
			head = NULL;
	
			cout << "程序运行完毕，释放头结点！" << endl;
		}
		}
};
```