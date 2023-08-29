### 二叉排序树

```c++
// 二叉排序树
# include<iostream>
# include<vector>
# include<string> 
using namespace std; 
/*
	二叉排序树:BST(Binary Sort(Search) Tree), 对于二叉排序树的任何一个非叶子结点，
	要求左子结点的值比当前结点的值小
	要求右子结点的值比当前结点的值大  
	尽量避免有重复结点 
*/ 

// 一个数组创建成对应的二叉排序树，并使用中序遍历二叉排序树

class Node{
	public:
		int value;
		Node* left;
		Node* right;
				
		Node(int value):value(value), left(NULL), right(NULL){}
		
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

// 创建二叉排序树
class BinarySortTree{
	public:	
		Node* root;
		BinarySortTree():root(NULL){}
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
		~BinarySortTree(){
			if(root != NULL){
				delete root;
				root = NULL;
			}
		} 
}; 

int main(){
	
	vector<int> arr = {7, 3, 10, 12, 5, 1, 9};
	BinarySortTree binarySortTree;
	for(int num:arr){
		binarySortTree.add(new Node(num));
	}
	binarySortTree.infixOrder(); 
	system("pause");
	return 0;
} 
 
```

### 二叉排序树的删除

```c++
// 二叉排序树的删除
/*
	二叉排序树的删除情况
	1.删除叶子结点
	2.删除只有一颗子树的结点
	3.删除有两颗子树的结点 
	思路:
	1.先找到要删除的结点 targetNode
	2.找到目标结点targetNode的父结点parent(要考虑是否存在父结点) 
	3.确定targetNode是parent的左子结点还是右子结点
	4.根据前面的情况来对应删除
	第一种情况: 
		parent->left = NULL  parent->right = NULL
	第二种情况:
		 4.2.1. 确定targetNode的子结点是左子结点还是右子结点
		 4.2.2. 确定targetNode是parent的左子结点还是右子结点
		 	 4.2.2.1. 如果targetNode是parent的左子结点 
			  		  targetNode的子结点是左子结点： parent->left = targetNode->left
			 4.2.2.2. 如果targetNode是parent的右子结点
			 		  targetNode的子结点是右子结点: parent->right = targetNode->right
			 4.2.2.3. 如果targetNode是parent的左子结点
			 		  targetNode的子结点是右子结点: parent->left = targetNode->right
			 4.2.2.4. 如果targetNode是parent的右子结点
			          targetNode的子结点是左子结点: parent->right = targetNode->left
	第三种情况:
		4.2.3.1 从targetNode的右子树找到最小的结点 用临时变量tempNode保存 
		4.2.3.2 targetNode->value = tempNode->value 删除tempNode 
		也可以从左子树中找最大的结点 
*/ 
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
} 


```

