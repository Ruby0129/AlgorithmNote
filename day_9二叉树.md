### 二叉树

```c++
// 树结构基础部分
// 为什么需要树这种数据结构
/*
	数组存储方式的分析
	优点:通过下标方式访问元素，速度快。对于有序数组，还可利用二分查找提高检索速度。
	缺点:如果要检索具体值，或者插入值会整体移动，效率较低。
	
	链式存储方式的分析
	优点:在一定程度上对数组存储方式有优化(比如:插入一个数值节点，只需要将插入节点链接到链表中即可，删除效率也很好)。
	缺点:在进行检索时，效率仍然很低，比如(检索某个值，需要从头节点开始遍历)。
	
	树存储方式的分析
	能提高数据存储、读取的效率，比如利用二叉排序树(Binary Sort Tree), 既可以保证数据的检索速度，同时也可以保证数据的插入、修	  改、删除的速度。
*/
/*
	树的常用术语
	节点
	根节点
	父节点
	子节点
	叶子节点:没有子节点的节点
	节点的权:节点值
	路径:从root节点找到该节点的路线
	层
	子树
	树的高度:最大层数
	森林:多颗子树构成森林
*/

/*
	二叉树
	1.每个节点最多只能有两个子节点
	2.二叉树的子节点分为左节点和右节点
	3.如果该二叉树的所有叶子节点都在最后一层，并且节点总数 = 2^n - 1, n为层数, 则我们称为满二叉树。
	4.如果该二叉树的所有叶子节点都在最后一层或者倒数第二层，而且最后一层的叶子节点在左边连续，导数第二层的叶子节点在右边连续，		称为完全二叉树。
*/
```

### 二叉树遍历

```c++
// 前序遍历: 先输出父节点，再遍历左子树和右子树
// 中序遍历: 先遍历左子树，在输出父节点，再遍历右子树
// 后序遍历: 先遍历左子树，再遍历右子树，最后输出父节点
```

### 定义二叉树

```c++
// 二叉树前序遍历 
# include<iostream>
# include<string>
using namespace std;

/*
	分析二叉树的前序、中序、后序的遍历步骤
	1.创建一颗二叉树
	2.前序遍历
	2.1 先输出当前节点
	2.2 如果左子节点不为空，则递归继续前序遍历。 
	2.3 如果右子节点不为空，则递归继续前序遍历。 
	
	3.中序遍历
	3.1 如果左子节点不为空，递归中序遍历 
	3.2 输出当前节点
	3.3 如果右子节点不为空，递归中序遍历 

	4.后序遍历
	4.1 如果当前左子节点不为空，递归后序遍历
	4.2 如果当前右子节点不为空，递归后序遍历
	4.3 输出当前节点 
*/

// 先创建HeroNode节点
class HeroNode{
	private:
		int no;
		string name;
		HeroNode *left = nullptr;
		HeroNode *right = nullptr;
	public:
		
		HeroNode(int no, string name){
			this->no = no;
			this->name = name;
		}
		
		void setLeft(HeroNode *node){
			this->left = node;
		}
		
		void setRight(HeroNode *node){
			this->right = node;
		}
		
		int getNo(){
			return this->no;
		}
		
		string getName(){
			return this->name;
		}
		
		// 输出节点 
		void printNode(HeroNode *node){
			cout << "id = " << node->getNo() 
				<< " name = " << node->getName() << endl;	
		}
				
		// 前序遍历
		void preOrder(){
			// 输出当前节点 
			printNode(this);
			// 递归向左子树前序遍历
			if(this->left != NULL){
				(this->left)->preOrder();
			}
			// 递归向右子树前序遍历 
			if(this->right != NULL){
				(this->right)->preOrder();
			} 
		} 
		
		// 中序遍历
		void infixOrder(){
			// 递归向左子树中序遍历
			if(this->left != NULL){
				(this->left)->infixOrder();
			}
			// 输出当前节点 
			printNode(this);
			// 递归向右子树中序遍历 
			if(this->right != NULL){
				(this->right)->infixOrder();
			} 
		}
		// 后序遍历
		void postOrder(){
			// 递归向左子树后序遍历 
			if(this->left != NULL){
				(this->left)->postOrder(); 
			}if(this->right != NULL){
			// 递归向右子树后序遍历 
				(this->right)->postOrder(); 
			}
			// 输出节点 
			printNode(this);
		} 
		 
		// 析构函数
		~HeroNode(){
			if(left != NULL){
				delete left;
				left = NULL;
			}
			if(right != NULL){
				delete right;
				right = NULL;
			}
		} 
}; 

// 定义二叉树
class BinaryTree{
	private:
		HeroNode *root = nullptr;
	public:
		BinaryTree(HeroNode *root){
			this->root = root; 
		}
		// 前序遍历
		void preOrder(){
			if(this->root != NULL){
				(this->root)->preOrder(); 
			}else{
				cout << "根节点为空!" << endl;
			}
		} 
		// 中序遍历
		void infixOrder(){
			if(this->root != NULL){
				(this->root)->infixOrder(); 
			}else{
				cout << "根节点为空!" << endl;
			}
		} 
		// 后续遍历
		void postOrder(){
			if(this->root != NULL){
				(this->root)->postOrder(); 
			}else{
				cout << "根节点为空!" << endl;
			}
		} 
		// 析构函数
		~BinaryTree(){
			if(root != NULL){
				delete root;
				root = NULL;
			}
		} 
}; 
 
```

### 前序遍历实现

```c++
// 前序遍历
void preOrder(){
	// 输出当前节点 
	printNode(this);
	// 递归向左子树前序遍历
	if(this->left != NULL){
		 (this->left)->preOrder();
	}
	// 递归向右子树前序遍历 
	if(this->right != NULL){
		(this->right)->preOrder();
	} 
} 
```

### 中序遍历实现

```c++
// 中序遍历
void infixOrder(){
	// 递归向左子树中序遍历
	if(this->left != NULL){
		(this->left)->infixOrder();
	}
	// 输出当前节点 
	printNode(this);
	// 递归向右子树中序遍历 
	if(this->right != NULL){
		(this->right)->infixOrder();
	} 
}
```

### 后序遍历实现

```c++
// 后序遍历
void postOrder(){
	// 递归向左子树后序遍历 
	if(this->left != NULL){
		(this->left)->postOrder(); 
	}if(this->right != NULL){
	// 递归向右子树后序遍历 
		(this->right)->postOrder(); 
	}
	// 输出节点 
	printNode(this);
} 
```

### 二叉树查找指定节点

```c++
// 前序遍历查找
HeroNode* preOrderSearch(int no){
	if(this->no == no){
		return this;
	}
	// 1.判断当前节点的左子节点是否为空，不为空，递归前序查找 
	HeroNode *resNode = nullptr;
	if(this->left != NULL){
		resNode = (this->left)->preOrderSearch(no);
	}
	if(resNode != NULL){
		return resNode;
	}
	if(this->right != NULL){
		resNode = (this->right)->preOrderSearch(no);
	}
	return resNode;
}
// 中序遍历查找
HeroNode* infixOrderSearch(int no){
	HeroNode *resNode = nullptr;
	if(this->left != NULL){
		resNode = (this->left)->infixOrderSearch(no);
	}
	if(resNode != NULL){
		return resNode;
	}
	if(this->no == no){
		return this;
	}
	if(this->right != NULL){
		resNode = (this->right)->infixOrderSearch(no);
	}
	return resNode;
} 
 // 后序遍历查找
HeroNode* postOrderSearch(int no){
	HeroNode *resNode = nullptr;
	if(this->left != NULL){
		resNode = (this->left)->postOrderSearch(no);
	}
	if(resNode != NULL){
	// 在左子树找到 
		return resNode;
	}
	// 左子树没找到，则向右子树递归进行查找 
	if(this->right != NULL){
		resNode = (this->right)->postOrderSearch(no);
	}
	if(resNode != NULL){
		return resNode;
	}
	// 左右子树都没找到，就比较当前节点是不是
	if(this->no == no){
		return this;
	}
	return resNode; 
}
```

### 二叉树删除节点

```c++
                                                                                                                    void delNode(int no){
                                                                                                                        //	思路:
                                                                                                                        //	首先进行:
                                                                                                                        //	如果树是空树或只有一个root节点，则等价于将二叉树置空  
                                                                                                                        //	1.因为我们的二叉树是单向的，所以判断当前节点的字节点是否需要删除。而不能去判断当前节点是不是需要删除的节点 
                                                                                                                        //	2.如果当前节点的左子节点不为空，并且左子节点就是要删除的节点，就将this->left = NULL;  返回结束递归删除 
                                                                                                                        //	3.如果当前节点的右子节点不为空，并且右子节点就是要删除的节点，就将this->right = NULL; 返回结束递归删除
                                                                                                                        //	4.如果3、4没有删除节点，就需要向左子树进行递归删除
                                                                                                                        //	5.如果左子树没有删除成功，向右子树进行删除
                                                                                                                        if((this->left != NULL) && (this->left)->no == no){
                                                                                                                                delete this->left;
                                                                                                                                this->left = NULL;
                                                                                                                                cout << " 删除节点成功 " << endl;
                                                                                                                                return; 
                                                                                                                        }
                                                                                                                        if((this->right != NULL) && (this->right)->no == no){
                                                                                                                            delete this->right;
                                                                                                                            this->right = NULL;
                                                                                                                            cout << "删除节点成功" << endl;
                                                                                                                            return;
                                                                                                                        }
                                                                                                                        // 向左子树进行递归
                                                                                                                        if(this->left != NULL){
                                                                                                                            (this->left)->delNode(no);		
                                                                                                                        }
                                                                                                                        if(this->right != NULL){
                                                                                                                            (this->right)->delNode(no);
                                                                                                                        } 			
                                                                                                                    }
```

### 顺序存储二叉树

```c++
// 顺序存储二叉树
// 从数据存储来看，数组存储方式和树的存储方式可以相互转换
// 即数组可以转换成树，树也可以转换成数组
# include<iostream>
# include<vector>
using namespace std;

/*
	顺序存储二叉树的特点:
	1.顺序二叉树通常只考虑完全二叉树
	2.第n个元素的左子节点为2*n + 1
	3.第n个元素的右子节点为2*n + 2
	4.第n个元素的父节点为(n-1)/2
	5.n:表示二叉树中的第几个元素(按0开始编号) 
*/ 

// 顺序存储二叉树  
class ArrBinaryTree{
	private:
		vector<int> arr; // 存储数据结点的数组 
	public:
		ArrBinaryTree(vector<int> arr){
			this->arr = arr;
		}
		// 顺序存储二叉树的前序遍历
		// index数组的下标 
		void preOrder(int index){
			if(arr.empty()){
				cout << "数组为空,不能按照二叉树的前序遍历" << endl;
			}
			// 输出当前的元素	
			cout << arr[index] << " ";
			// 向左递归遍历 
			if((2*index + 1) <= arr.size()-1){
				preOrder(2*index+1);
			}
			// 向右递归遍历 
			if((2*index+2) <= arr.size()-1){
				preOrder(2*index+2);
			}
		}
		
		void preOrder(){
			this->preOrder(0); 
		}
		~ArrBinaryTree(){
			
		} 
};


int main(){
	
	vector<int> arr = {1, 2, 3, 4, 5, 6, 7};
	
	ArrBinaryTree *arrBinaryTree = new ArrBinaryTree(arr);
	arrBinaryTree->preOrder();
	
	if(arrBinaryTree != NULL){
		delete arrBinaryTree;
		arrBinaryTree = NULL;
	}	
	
	system("pause");
	return 0;
}
```

### 线索二叉树

```c++
// 因为线索化后，各个结点可以通过线性方式遍历，无需使用递归方式。 
// 中序线索二叉树
class ThreadedBinaryTree{
	private:
		// 为了实现线索化，需要创建指向当前结点的前驱结点的指针
		// 在递归进行线索化时，pre总是保留前一个结点 
		HeroNode *pre =nullptr; 
		HeroNode *root = nullptr;
	public:
		ThreadedBinaryTree(HeroNode *root){
			this->root = root; 
		}
		void threadedNodes(){
			this->threadedNodes(root);
		}
		// 遍历线索化二叉树的方法
		void list(){
			// 定义遍历，存储当前遍历的结点，从root开始
			HeroNode *node = this->root;
			while(node != NULL){
				// 循环找到leftType == 1的结点 
				// 后面随着遍历而变化 当leftType==1时，该结点是按照线索化处理后的有效结点 
				while(node->getLeftType() == 0){
					node = node->getLeft();
				}
				// 打印当前结点
				cout << node->getNo() << " " << node->getName() << endl;
				// 如果当前结点的右指针指向的是后继结点，就一直输出
				while(node->getRightType() == 1){
					// 获取当前结点的后继节点
					node = node->getRight();
					cout << node->getNo() << " " << node->getName() << endl; 
				}
				// 替换遍历的结点
				node = node->getRight(); 
			} 
		} 
		// 对二叉树进行中序线索化的方法
		void  threadedNodes(HeroNode *node){
			// node就是当前需要线索化的结点
			if(node == NULL){
				return;
			}
			// 线索化左子树
			threadedNodes(node->getLeft());
			// 线索化当前结点
			// 处理当前结点的前驱结点
			if(node->getLeft() == NULL){
				// 当前结点的左指针
				node->setLeft(pre);
				// 修改当前结点的左指针类型 指向前驱结点 
				node->setLeftType(1);
			}
			// 处理后继结点
			if(pre!=NULL && pre->getRight()==NULL){
				// 让前驱结点的右指针指向当前结点 
				pre->setRight(node);
				// 修改前驱结点的右指针类型 
				pre->setRightType(1);
			}
			// !!! 每处理一个结点后，让当前结点称为下一个结点的前驱结点 
			this->pre = node; 
			// 线索化右子树 
			threadedNodes(node->getRight());
		}
};
```

