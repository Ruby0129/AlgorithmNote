### **平衡二叉树(AVL树)**

```c++
// 平衡二叉树
# include<iostream>
# include<vector>
using namespace std;

/*
	平衡二叉树也叫平衡二叉搜索树(Self=balancing binary search tree)又被称为AVL树，可以保证查询效率较高
	平衡二叉树的特点
		1. 它是一棵空树或它的左右两个子树的高度差的绝对值不超过1。 
		2. 左右两个子树都是一棵平衡二叉树。 
	平衡二叉树的常用实现方法有红黑树、AVL、替罪羊树、Treap、伸展树等。 
*/ 

/*
右子树比左子树高度大于1 进行左旋转:
	1.创建一个新的结点newNode,值等于当前根结点的值
	2.把新结点的左子树设置为当前结点的左子树 newNode->left = this->left
	3.把新结点的右子树设置为当前结点右子树的左子树 newNode->right = this->right->left
	4.把当前结点的值换位右子结点的值 this->value = this->right->value
	5.把当前结点的右子树设置为右子树的右子树  this->right = this->right->right
	6.把当前结点的左子树设置为新结点 this->left = newNode 
*/ 

/*
左子树比右子树高度大于1 进行右旋转:
	1.创建一个新的结点newNode,值等于当前根结点的值  
	2.把新的结点的右子树设为当前结点的右子树 newNode->right = this->right
	3.把新的结点的左子树设为当前结点左子树的右子树 node->left = this->left->right 
	4.把当前结点的值设为左子结点的值 this->value = this->left->value
	5.把当前结点的左子树设为左子树的左子树 this->left = this->left->left 
	6.把当前结点的右子树设置为新结点  this->right = newNode


*/
/*
某些情况下，单旋转(即一次旋转)就可以将非平衡二叉树转成平衡二叉树， 
但是在某些情况下，单旋转不能完成平衡二叉树的转换。

问题分析:
1.当符合右旋转的条件时
2.如果它的左子树的右子树高度大于它的左子树的高度
3.先对当前这个结点的左结点进行左旋转
4.再对当前结点进行向右旋转的操作 
  
*/

int max(int a, int b){
	if(a < b){
		return b;
	}
	return a;
}

class Node{
	public:
		int value;
		Node* left;
		Node* right;		
		Node(int value):value(value), left(NULL), right(NULL){}
		
		
		// 返回左子树的高度
		int leftHeight(){
			if(this->left == NULL){
				return 0;
			}
			return this->left->height();
		}
		
		// 返回右子树的高度
		int rightHeight(){
			if(this->right == NULL){
				return 0;
			}
			return this->right->height();
		}
			
		// 返回以该结点为根结点的树的高度
		int height(){
			return max(this->left == NULL ? 0 : this->left->height(),
					  this->right == NULL ? 0 : this->right->height()) + 1;  
		} 
		// 左旋转的方法
		void leftRotate(){
			// 1.创建新的结点，值为当前根结点的值
			Node *newNode = new Node(this->value);
			// 2.将新的结点的左子树，设为当前结点的左子树
			newNode->left = this->left;
			// 3.将新的结点的右子树，设为当前结点右子树的左子树
			newNode->right = this->right->left;
			// 4.将当前结点的值设为右子结点的值
			this->value = this->right->value;
			// 5.将当前结点的右子树设为当前结点右子树的右子树
			this->right = this->right->right;
			// 6.将当前结点的左子树设为新节点
			this->left = newNode; 
		} 
		// 右旋转的方法
		void rightRotate(){
			// 1.创建新的结点，值为当前根结点的值 
			Node *newNode = new Node(this->value);
			// 2.将新的结点的右子树，设为当前结点的右子树
			newNode->right = this->right;
			// 3.将新的结点的左子树，设为当前结点左子树的右子树 
			newNode->left = this->left->right;
			// 4.将当前结点的值设为左子结点的值
			this->value = this->left->value;
			// 5.将当前结点的左子树设为当前结点左子树的左子树
			this->left = this->left->left;
			// 6.将当前结点的右子树设为新结点
			this->right = newNode; 
		} 
		// 中序遍历
		void infixOrder(){
			if(this->left != NULL){
				this->left->infixOrder();
			}
			cout << "value = " << this->value << " ";
			if(this->right != NULL){
				this->right->infixOrder();
			}
		} 
		
		// 查找要删除的结点
		Node* search(int value){
			if(value == this->value){
				// 找到就是该结点
				return this; 
			}else if(value < this->value){
				// 如果查找的值小于当前结点，递归查找左子树
				// 如果左子结点为空
				if(this->left == NULL){
					return NULL;
				} 
				return this->left->search(value); 
			}else{
				// 如果右子结点为空
				if(this->right == NULL){
					return NULL;
				}
				return this->right->search(value); 
			}
		}  
		// 查找要删除结点的父节点
		Node* searchParent(int value){
			//  如果当前的结点就是要删除的结点的父结点，就返回 
			if((this->left != NULL && this->left->value == value)||(this->right != NULL && this->right->value == value)){
				return this;
			}
			// 如果查找的值小于当前结点的值，并且当前结点不为空
			if(value < this->value && this->left != NULL){
				return this->left->searchParent(value);
			}else if(value >= this->value && this->right != NULL){
				return this->right->searchParent(value);
			}else{
				return NULL; // 没有找到父结点 
			} 			
		} 
		
		// 添加结点的方法
		void add(Node* node){
			// 递归的方式添加结点，注意满足二叉排序树的需求
			if(node == NULL){
				return;
			}
			// 判断传入的结点的值和当前子树的根节点的值的关系
			if(node->value < this->value){
				// 如果当前子树左子结点为空，直接挂入结点 
				if(this->left == NULL){
					this->left = node;
					return; 
				}else{
					// 否则递归左子树添加 
					this->left->add(node);
				} 
			}
			if(node->value >= this->value){
				// 如果当前子树右子结点为空，直接挂入结点
				if(this->right == NULL){
					this->right = node;
					return;
				}else{
					// 否则递归右子树添加 
					this->right->add(node); 
				}
			} 
			// 当添加完一个结点后，如果:(右子树的高度-左子树的高度) > 1 => 左旋转
			if(this->rightHeight() - this->leftHeight() > 1){
				// 如果它的右子树的左子树的高度大于它的右子树的右子树的高度
				if(this->right != NULL && this->right->leftHeight() > this->right->rightHeight()){
					// 先对它的右子树进行右旋转
					this->right->rightRotate(); 
				} 
				// 再对当前结点进行左旋转 
				this->leftRotate();
				return;  
			} 
			// 当添加完一个结点后，如果:(左子树的高度- 右子树的高度) > 1 => 右旋转
			if(this->leftHeight() - this->rightHeight() > 1){
				// 如果它的左子树的右子树的高度大于它的左子树的左子树的高度
				if(this->left != NULL && this->left->rightHeight() > this->left->leftHeight()){
					 // 先对它的左子树进行左旋转
					 this->left->leftRotate(); 
				}
				// 再对当前结点进行右旋转 
				this->rightRotate();
				return;
			}
			 
		}
		
		~Node(){
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

// 创建AVLTree
class AVLTree{
	public:	
		Node* root;
		AVLTree():root(NULL){}
		
		// 查找要删除的结点
		Node* search(int value){
			if(root == NULL){
				return NULL;
			}
			return root->search(value);
		} 
		
		// 查找要删除结点的父结点
		Node* searchParent(int value){
			if(root == NULL){
				return NULL;
			}
			return root->searchParent(value); 
		}
		// 
		int delRightTreeMin(Node *node){
			// 传入的结点(当作二叉排序树的根结点)
			// 返回的以node为根结点的二叉排序树的最小结点的值
			// 删除以node为根节点的二叉排序树的最小结点
			Node *target = node;
			// 循环的查找左结点
			while(target->left != NULL){
				target = target->left;
				cout << target->value << endl;
			}
			// 这时target就指向了最小结点
			// 删除最小结点 
			delNode(target->value); 
			return target->value;
		} 
		 // 删除结点
		 void delNode(int value){
		 	 if(root == NULL){
		 		  cout << "结点为空，无法删除!!! " << endl; 
				  return;
			 }
			  // 如果发现当前这颗二叉排序树只有一个结点
			  if(root->left == NULL && root->right == NULL&& root->value == value){
			  		if(root != NULL){
			  			root = NULL;
					}
				return;
			  } 
			  
			 // 1.先找到要删除的结点targetNode
			 Node *targetNode = search(value); 
		 	 if(targetNode == NULL){
		 	 	cout << "没有找到该结点" << endl;
			  	return;
			  }
			  // 去找到targetNode的父结点
			  Node* parent = this->searchParent(value);
			  // 如果发现targetNode没有父结点，且targetNode有子树 
			  if(parent == NULL){
				if(targetNode->left != NULL && targetNode->right == NULL){
					// 如果parent有左子树，无右子树 
					targetNode = targetNode->left;
				}else if(targetNode->right != NULL && targetNode->left == NULL){
					// 如果parent有右子树，无左子树 
					targetNode = targetNode->right;
				}else if(targetNode->left != NULL && targetNode->right != NULL){
					// 左右子树都不为空 找到右子树中最小的结点
					int min_val = this->delRightTreeMin(targetNode->right);
					targetNode->value = min_val; 
				}
				return;
			  }
			  cout << "targetNode = " << targetNode->value <<" "
			  	   << "parentNode = " << parent->value << endl; 
			  // 如果要删除的结点是叶子结点
			  if(targetNode->left == NULL && targetNode->right == NULL){
			  		// 判断targetNode是父结点的左子结点还是右子结点
					  if(parent->left != NULL && parent->left == targetNode){
					  	// 是父结点的左子结点
						 parent->left = NULL; 
					  }else if(parent->right != NULL && parent->right == targetNode){
					  	// 是父结点的右子结点
						  parent->right = NULL; 
					  }
			  }else if(targetNode->left != NULL && targetNode->right != NULL){
			  	   // 删除有两棵子树的结点
				   // 从targetNode的右子树中找到最小的结点
				    cout << " targetNode有两棵子树 " << endl; 
					int min_value = this->delRightTreeMin(targetNode->right);
					cout << "min_value = " << min_value << endl; 
				    targetNode->value = min_value;
			  }else{
				  // 删除有一棵子树的结点 
				  cout << "targetNode只有一棵子树" << endl; 
			  	  if(targetNode->left != NULL){
				  		// 如果要删除的结点有左子结点
						if(parent->left != NULL && parent->left == targetNode){
			  	  			// 如果targetNode是parent的左子结点
			  	  			cout << "targetNode有左子树, targetNode是parent的左子结点" << endl; 
							parent->left = targetNode->left;
						}else if(parent->right != NULL && parent->right == targetNode){
							// 如果targetNode是parent的右子结点
			  	  			cout << "targetNode有左子树, targetNode是parent的右子结点" << endl; 							
							 parent->right = targetNode->left;							 
						} 
					}else if(targetNode->right != NULL){
						// 如果要删除的结点有右子结点
						if(parent->left != NULL && parent->left == targetNode){
							// 如果targetNode是parent的左子结点 
			  	  			cout << "targetNode有右子树, targetNode是parent的左子结点" << endl; 							
							parent->left = targetNode->right;							
							
						}else if(parent->right != NULL && parent->right == targetNode){
							// 如果targetNode是parent的右子结点	
			  	  			cout << "targetNode有右子树, targetNode是parent的右子结点" << endl; 							
							parent->right = targetNode->right;								
						} 
					}			  			  	
			  }
			  //    
		 } 
		 
		// 添加结点
		void add(Node *node){
			if(this->root == NULL){
				root = node; // 如果为空树，node为根结点 
				return;
			} 
			root->add(node);
		}
		// 中序遍历
		void infixOrder(){
			if(root == NULL){
				cout << "数为空不能遍历" << endl;
				return;
			}
			cout << "中序遍历效果" << endl;
			root->infixOrder();
			cout << endl;
		} 
		~AVLTree(){
			if(root != NULL){
				delete root;
				root = NULL;
			}
		} 
};  

int main(){
	
	vector<int> arr = {10,11,7,6,8,9};	
	AVLTree avlTree;
	for(int num:arr){
		avlTree.add(new Node(num));
	}
	avlTree.infixOrder();	
	// 左旋处理
	cout << "树的高度:"<<(avlTree.root)->height()<<endl;
	cout << "树的左子树高度:"<<(avlTree.root)->leftHeight()<<endl;
	cout << "树的右子树高度:"<<(avlTree.root)->rightHeight()<<endl;
	cout << "根结点: " << (avlTree.root)->value << endl ;	 
	cout << "根结点的左子结点: " << (avlTree.root)->left->value << endl ;
	cout << "根结点的右子结点: " << (avlTree.root)->right->value << endl ;	
	system("pause");
	return 0;
} 
```



![](D:\CandCpp\note\数据结构与算法\img\树\AVL左旋转.JPG)

![](D:\CandCpp\note\数据结构与算法\img\树\AVL右旋转.JPG)

![](D:\CandCpp\note\数据结构与算法\img\树\AVL双旋转.JPG)

### **多路查找树**

```c++
// 二叉树的问题分析
/*
	二叉树的操作效率较高，但也存在问题
	二叉树需要加载到内存的，如果二叉树的节点少，没有什么问题，但是如果二叉树的节点很多，就会存在很多问题。
	1.在构建二叉树时，需要多次进行i/o操作(海量数据存在数据库或文件中)，节点海量，构建二叉树时，速度有影响。
	2.节点海量，也会造成二叉树的高度很大，会降低操作速度。
*/

// 多叉树
// 1.允许每个节点可以有更多的数据项和更多的子结点。
// 2，多叉树通过重新组织结点，减少树的高度，能对二叉树进行优化。
```

### B-树

```c++
// B-tree
// B树通过重新组织结点，降低树的高度，并且减少i/o读写次数来提升效率。
// 文件系统及数据库系统的设计者利用了磁盘预读原理，将一个结点的大小设为等于一个页(页的大小通常为4k)，这样每个节点只需要一次I/O    就可以完全载入。
// 将树的度M设置为1024，在600亿个元素中最多只需要4次I/O操作就可以读取到想要的元素，B树广泛应用于文件存储系统以及数据库系统      中。

// 2-3树基本介绍
// 2-3树是最简单的B树结构，有如下特点:
// 1. 2-3树的所有叶子结点都在同一层(只要是B树就满足这个条件)。
// 2. 有两个子结点的结点叫做二结点，二结点要么没有子结点，要么有两个子结点。
// 3. 有三个子结点的结点叫做三结点，三结点要么没有子结点，要么有三个子结点。
// 4. 2-3树是由二结点和三结点构成的树。

// B-树
// 1.B-树的阶:结点的最多子结点个数。
// 2.B-树的搜索，从根结点开始，对结点内的关键字(有序)序列进行二分查找，如果命中则结束，负责进入查询关键字所属范围的二子结点；
// 重复，知道所对应的儿子指针为空，或已经是叶子节点。
// 3.关键字集合分布在整棵树中，即叶子结点和非叶子结点都存放数据。
// 4.搜索有可能在非叶子结点结束。
// 5.其搜索性能等价于在关键字全集内做一次二分查找。
```

### B+树

```c++
// 1.B+树的搜索与B树基本相同，区别是B+树只有到达叶子结点才命中(B树可以在非叶子结点命中)。其性能也等价于在关键字全集内做一次二分查找。
// 2.B+树的所有关键字都出现在叶子结点的链表中(即数据只能在叶子结点(也叫稠密索引))，且链表中的关键字(数据)恰好是有序的。
// 3.不可能在非叶子结点名中。
// 4.非叶子结点相当于时叶子结点的索引(稀疏索引)，叶子结点相当于时存储(关键字)数据的数据层。
// 5.更适合文件索引系统。
// 6.B树和B+树各有自己的应用场景。
```

