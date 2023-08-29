### 哈希表

```c++
// 哈希表 
#include<iostream>
#include<string>
#include<vector>
using namespace std;

/*
	哈希表的基本介绍
	散列表(Hashtable, 也叫哈希表), 是根据关键码值(Key,value)而直接进行访问的数据结构。
	也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做散列函数，存放记录的数组叫做散列表。 
*/

/*
	使用哈希表来管理雇员信息 
*/ 

// 表示一个雇员
class Emp{
	public:
		int id;
		string name;
		Emp *next = nullptr;
		// 构造函数
		Emp(int id, string name){
			this->id = id;
			this->name = name;
		}
		
		// 析构函数
		~Emp(){
			if(this->next != NULL){
				delete this->next;
				this->next = NULL;
			}
		} 
};

// 表示链表
class EmpLinkedList{
	private:
		// 头指针指向第一个Emp，这个链表的head是有效的 
		Emp *head = nullptr;
	public:
		// 构造函数
		// 添加雇员到列表
		// 1.假定，当添加雇员时，id是自增长的，即id得分配总是从小到大
		// 因此将雇员添加到本链表的最后即可
		void add(Emp *emp){
			// 如果是添加第一个雇员 
			if(this->head == NULL){
				head = emp;
				return; 
			}
			// 如果不是第一个雇员，则使用一个辅助得指针帮助定位到最后
			Emp *curEmp = head;
			while(curEmp->next != NULL){
				// 说明还没有到最后一个
				 curEmp = curEmp->next;
			}
			// 退出循环后直接加入链表 
			curEmp->next = emp; 
		}
		
		// 遍历链表的雇员信息
		void list(int no){
			if(head == NULL){
				// 链表为空
				cout << "当前第" << no << "条链表为空" << endl;
				return; 
			}
				cout << "当前第" << no << "条链表信息为:" << endl;
			Emp *curEmp = head;
			while(true){
				cout << " id = " << curEmp->id << " name = " << curEmp->name << endl;
				if(curEmp->next == NULL){
					// 说明已经是最后一个节点 
					break;
				}
				curEmp = curEmp->next;
			}
		}
		// 根据id查找雇员
		// 如果查找到返回Emp
		// 未找到返回null 
		void findEmpById(int id, int index){
			if(head == NULL){
				cout << "链表空" << endl;
				return;
			}
			Emp *curEmp = head;
			while(true){
				if(curEmp->id == id){
					// curEmp就指向要查找得员工 
				cout << "在第" << index + 1 << "条链表中找到雇员" << curEmp->name << endl;
				break;  
				}
				if(curEmp->next == NULL){
					// 遍历当前链表没有找到该雇员
				cout << "未找到雇员" << endl;
				}
				curEmp = curEmp->next; 
			}
		} 
		// 析构函数
		~EmpLinkedList(){
			if(this->head != NULL){
				delete head;
				this->head = NULL;
			}
		} 
		 
}; 

// 创建HashTab 管理多条链表
class HashTab{
	private:
		vector<EmpLinkedList> empLinkedListArray;
		int size; // 表示多少个链表 
		
	public:
		// 构造函数
		HashTab(int size){
			// 初始化empLinkedListArray
			this->size = size;
			this->empLinkedListArray.resize(size);
		}
		// 添加雇员
		void add(Emp *emp){
			// 根据员工的id，得到该员工应当添加到哪条链表
			 int empLinkedListNo = hashFun(emp->id);
			 // 将emp添加到对应得链表中
			 (this->empLinkedListArray[empLinkedListNo]).add(emp);
		}
		void list(){
		 // 遍历所有的链表
		 int i = 1;
		 for(auto emplist : this->empLinkedListArray){
		 	emplist.list(i++);
		 } 	
		}
		// 根据查找的id，寻找雇员
		void findEmpById(int id){
			// 使用散列函数确定到哪条链表查找
			int empLinkedListNo = hashFun(id);
			empLinkedListArray[empLinkedListNo].findEmpById(id, empLinkedListNo);
		} 
		// 编写散列函数，使用简单取模法
		int hashFun(int id){
			return id % size; 
		} 
 
}; 

int main(){
	
	// 创建哈希表
	HashTab hashTab(7); 
	
	// 菜单
	int key = -1;
	int id = -1;
	string name = ""; 
	while(true){
		cout << "1:添加雇员" << endl;	
		cout << "2:显示雇员" << endl;
		cout << "3.查找雇员" << endl; 
		cout << "0:退出系统" << endl;
		cin >> key; 
		switch(key){
			case 1:
				{
					cout << "输入id" << endl;
					cin >> id;
					cout << "输入姓名" << endl;
					cin >> name;
					// 创建一个雇员
					Emp *emp = new Emp(id, name);
					hashTab.add(emp);
					break;	
				}
			case 2:
				hashTab.list();
				break;
			case 3:
				 {
				 	int find_id = 0;
				 	cout << "请输入要查找的雇员Id:" << endl;
				 	cin >> find_id;
				 	hashTab.findEmpById(id);
				 }
			case 0:
				system("pause");
				return 0;
				break;
			default:
				cout << "输入有误，请重新输入" << endl;
				system("pause");
				system("cls"); 
				break; 
		}
		system("pause");
		system("cls"); 
	}
	
	
	system("pause");
	return 0;	
} 
```

