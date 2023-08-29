### 堆排序

```c++
# include<iostream>
# include<vector>
# include<string>
using namespace std;
 
// 大顶堆和小顶堆

// 堆排序是利用堆这种数据结构而设计的一种排序算法
// 堆排序是一种选择排序，它的最坏、最好、平均时间复杂度均为0(nlogn),是不稳定排序
/*
	堆是具有以下性质的完全二叉树
	1.每个结点的值都大于或等于其左右孩子的结点的值，称为大顶堆
	2.没有要求结点的左孩子的值和右孩子的值的大小关系。
	3.每个结点的值都小于或等于其左右孩子结点的值，称为小顶堆。 
	大顶堆:arr[i] >= arr[2*i+1] && arr[i] >= arr[2*i+2] // i对应第几个结点，i从0开始 
	小顶堆: arr[i] <= arr[2*i+1] && arr[i] <= arr[2*i+2]
	
	升序用大顶堆，降序用小顶堆 
*/ 

/*
	堆排序基本思想
	1.对待排序序列构成一个大顶堆
	2.此时，整个序列的最大值就是堆顶的根节点
	3.将其与末尾元素进行交换，此时末尾元素就是最大值
	4.然后将剩下n-1个元素重新构造成一个堆，这样就会得到n个元素的次小值。
	5.如此反复执行，便能得到一个有序序列了。 

*/ 
// 将一个数组(二叉树)调整成一个大顶堆
void adjustHeap(vector<int> &arr, int i, int length){
	// arr待调整的数组 i非叶子结点在数组中的索引 length对多少个元素进行调整
	// length是在逐渐减少
	int temp = arr[i]; // 取出当前元素的值
	// 开始调整
	// k是i结点的左子结点 
	for(int k = i * 2 + 1; k < length; k = k * 2 + 1){
		if(k+1 < length && arr[k] < arr[k+1]){
			// 说明左子结点小于右子结点的值
			k++; // k指向右子结点 
		}
		if(arr[k] > temp){
			// 如果子结点大于父结点
			arr[i] = arr[k]; //把较大的值赋给当前结点 
			i = k; //  i指向k，继续循环比较 
		}else{
			break;
		} 
	}
	// 当for循环结束后，已经将i为父结点的树的最大值，放在了最顶(局部 以i为父节点)
	arr[i] = temp; // 将temp放到调整后的位置 
} 

// 最后一个非叶结点，就是最后一个结点的父节点 arr.size/2 - 1
void HeapSort(vector<int> &arr){
	// 第一个循环从下往上调整出一个大顶堆
	// 第二个循环只需要从根节点开始调整就可以了 
	int temp = 0;
	// 堆排序方法
	//	升序 选择大顶堆 
	cout << "堆排序" << endl;
	// arr.size()/2 - 1是最后一个非叶子结点，前面的就都是非叶子结点了 
	for(int i = arr.size()/2 - 1; i >= 0; i--){
		adjustHeap(arr, i, arr.size());
	} 
	// 已经是大顶堆了要交换
	for(int j=arr.size()-1; j>0;j--){
		temp = arr[j];
		arr[j] = arr[0];
		arr[0] = temp;
		adjustHeap(arr, 0, j);
	} 
		
} 

inline ostream&
operator << (ostream &os, const vector<int> arr){
	for(auto elem:arr){
		cout << elem << " ";
	}
	cout << endl;
}

int main(){
	
	vector<int> arr{4, 6, 8, 5, 9};
	
	cout << "排序前" << endl;
	cout << arr << endl; 
	cout << "堆排序结果" << endl;
	HeapSort(arr); 
	cout << arr << endl;	 
	
	system("pause");
	return 0;
}
```

### 霍夫曼树

```c++
# include<iostream>
# include<vector> 
# include<string>
# include<algorithm> 
using namespace std;
// 赫夫曼树
/*
	1.给定n个权值作为n个叶子结点，构造一棵二叉树，若该树的带权路径长度(wpl)达到最小，称这样的二叉树为最优二叉树，哈夫曼树(Huffman Tree)
	2.赫夫曼树是带权路径长度最短的树，权值较大的结点离根较近。 
*/ 

/*
	构建赫夫曼树的步骤:
	1.从小到大进行排序，将每一个数据，每个数据都是一个结点，每个结点可以看成是一颗最简单的二叉树。
	2.取出根节点权值最小的两棵二叉树
	3.组成一棵新的二叉树，该树的二叉树的根节点的权值是前面两棵二叉树根节点权重的和
	4.再将这棵新的二叉树，以根节点的权值大小再次排序，不断重复1-2-3-4的步骤，直到数列中，所有的数据都被处理，就得到一棵赫夫曼树。 
*/

inline ostream&
operator << (ostream &os, const vector<int> arr){
	for(auto elem:arr){
		cout << elem << " ";
	}
	cout << endl;
}

class Node{

public:
	int value; // 结点权值
	Node *left = nullptr; // 指向左子结点的指针 
	Node *right = nullptr; // 指向右子结点的指针
		
	Node(int value){
		this->value = value;
	}

	void printNode(){
		cout << "Node [" << this->value << "]" << endl;
	}
	// 前序遍历
	void preOrder(){
		this->printNode();
		if(this->left != NULL){
			this->left->preOrder();
		}
		if(this->right != NULL){
			this->right->preOrder();
		}
	} 
	~Node(){
		if(this->left != NULL){
			delete left;
			left = NULL;
		}
		if(this->right != NULL){
			delete right;
			right = NULL;
		}
	} 
	
}; 
void preOrder(Node *root){
	if(root == NULL){
		cout << "霍夫曼树为空" << endl;
		return;
	}
	root->preOrder(); 
}
void PopSort(vector<Node*> &nodes){
	Node *temp = nodes[0];
	for(int i=0; i<nodes.size(); ++i){
		for(int j=0; j<nodes.size()-i-1; ++j){
			if(nodes[j]->value >= nodes[j+1]->value){
				temp = nodes[j];
				nodes[j] = nodes[j+1];
				nodes[j+1] = temp;
			} 
		}
	} 
} 

// 创建霍夫曼树的方法 
// 返回霍夫曼树的根结点 
Node* createHuffmanTree(vector<Node*> nodes){
	// 第一步 为了操作方便
	// 遍历arr 将arr的每个元素构建成一个Node
	while(nodes.size() > 1){
		// 排序 从小到大 
		PopSort(nodes);
		// 取出根节点权值最小的两棵二叉树
		// 取出权值最小的结点(二叉树)
		Node *left = nodes[0];
		// 取出权值第二小的结点(二叉树) 
		Node *right = nodes[1];
		// 构建一颗新的二叉树
		Node *parent = new Node(left->value + right->value);
		parent->left = left;
		parent->right = right;
		// 从Arr中删除处理过的二叉树
		nodes.erase(nodes.begin());
		nodes.erase(nodes.begin());
		// 将parent加入到nodes
		nodes.push_back(parent);		
	}
	// 返回赫夫曼树的root结点
	return nodes[0];	 
} 

int main(){
	
	vector<int> arr{13, 7, 8, 3, 29, 6, 1};		 
	vector<Node*> nodes;
	for(auto value : arr){
		nodes.push_back(new Node(value));		
	}
	Node *root = createHuffmanTree(nodes);
	preOrder(root);
//	nodes.erase(nodes.begin());
//	nodes.erase(nodes.begin());	
//	for(Node *node : nodes){
//		node->printNode();
//	}
	system("pause");
	return 0;
}
```

### 霍夫曼编码

```c++
// 广泛用于数据文件压缩。压缩了通常在20%~90%之间
// 霍夫曼编码是可变字长编码的一种
# include<iostream>
# include<vector>
# include<string>
# include<map>
typedef  unsigned int byte;
using namespace std;
//  赫夫曼编码
/*
	赫夫曼编码原理
	1.传输字符串
		i like like like java do you like a java
	2.统计各个字符出现的个数
	3.按照上面字符出现的次数构建一棵赫夫曼树，次数作为权值
	4.根据赫夫曼树，给各个字符规定编码 向左的路径为0 向右的路径为1 
*/ 
/*
	思路:
	1. Node{data 存放数据  weight权重  left  right}
	2.得到字符串对应的byte数组
	3.编写方法，将准备构建霍夫曼树的Node结点，放在list中
	4.通过list创建对应的赫夫曼树 
*/
// 创建Node,带数据和权值




class Node{
public: 
	char data; // 存放数据本身 'a'=97 ''=32 
	int weight; //权值，表示字符出现的次数
	Node* left = NULL;
	Node* right = NULL;

	Node(char data, int weight):data(data),weight(weight){}
	
	void printNode(){
		cout << "Node[data = " << this->data << " weight = " << this->weight << "]" << endl;
	}
	// 前序遍历 
	void preOrder(){
		this->printNode();
		if(this->left != NULL){
			this->left->preOrder();
		}
		if(this->right != NULL){
			this->right->preOrder();
		} 
	}
	~Node(){
		if(this->left != NULL){
			delete left;
			left = NULL;
		}
		if(this->right != NULL){
			delete right;
			right = NULL;
		}
	}
}; 


void PopSort(vector<Node*> &nodes){
	Node *temp = nodes[0];
	for(int i=0; i<nodes.size(); ++i){
		for(int j=0; j<nodes.size()-i-1; ++j){
			if(nodes[j]->weight >= nodes[j+1]->weight){
				temp = nodes[j];
				nodes[j] = nodes[j+1];
				nodes[j+1] = temp;
			} 
		}
	} 
} 

void preOrder(Node* root){
	if(root != NULL){
		root->preOrder(); 
	}
} 

vector<Node*> getNodes(const string bytes){
	vector<Node*> nodes;
	// 存储每一个char出现的次数
	 map<char, int> counts;
	 for(const char byte : bytes){
	 	if(counts.count(byte) == 0){
	 		counts.insert(pair<char, int>(byte, 1));
		 }else{
		 	counts[byte]++;
		 }
	 }
	  // 把每个键值对转成一个Node对象加入到nodes
	  for(pair<char, int>elem : counts){
	  	nodes.push_back(new Node(elem.first, elem.second));
	  }
//	  for(int i=0;i<nodes.size();i++){
//	  	nodes[i]->printNode();
//	  }
	return nodes;
}

// 通过vector创建霍夫曼树
Node* createHuffmanTree(vector<Node*> nodes){
	while(nodes.size() > 1){
		// 排序
		PopSort(nodes);
		// 取出最小 和 第二小的结点
		Node *leftNode = nodes[0];
		Node *rightNode = nodes[1];
		// 构建新的二叉树 没有data只有权值 
		Node *parent = new Node(NULL, leftNode->weight + rightNode->weight);
		parent->left = leftNode;
		parent->right = rightNode;
		nodes.erase(nodes.begin());
		nodes.erase(nodes.begin());
		nodes.push_back(parent);
	}
	return nodes[0];
} 

// 生成霍夫曼树对应的霍夫曼编码表
/*
	思路:
	1.将霍夫曼编码表存放在Map<char, String>形式
	2.生成霍夫曼编码时，需要去拼接路径，定义一个String,存储某个叶子结点的路径 

*/ 
// map<char, string> haffmanCode;
// string stringBuilder;
 
 
 // code左子结点是0 右子结点是1
 // stringBuilder拼接路径 
map<char, string> haffmanCode;
string stringBuilder = "";
 void getCodes(Node *node, const char* code, string stringBuilder, map<char, string>&haffmanCode){
 	// 将传入的code加入到stringBuilder
 	string stringBuilder2 = stringBuilder + code;
	 if(node != NULL){
	 	// 判断当前node是叶子结点还是非叶子结点
		 if(node->left != NULL || node->right != NULL){
		 	// 非叶子结点
			 // 递归处理
			 // 向左
			 getCodes(node->left, "0", stringBuilder2, haffmanCode);
			 // 向右
			 getCodes(node->right, "1", stringBuilder2, haffmanCode); 
		 }else{
		 	// 找到了某个叶子结点 
			haffmanCode.insert(pair<char, string>(node->data, stringBuilder2));
		 }
	 } 
 }

// 重载getCodes 
void getCodes(Node *root){
	if(root == NULL){
		cout << "哈夫曼树为空" << endl;
		return; 
	}
	// 处理root的左子树
	getCodes(root->left, "0", stringBuilder, haffmanCode);
	// 处理root的右子树 
	getCodes(root->right, "1", stringBuilder, haffmanCode);
}


// 将一个字符串 通过生成的霍夫曼编码表，返回一个霍夫曼编码处理后的数组
// 每8位一个byte 
byte* zip(string bytes, map<char, string> huffmanCodes, int &len){

	// 1.利用huffmanCodes将bytes转成霍夫曼对应的字符串 
	string stringBuilder = "";
	for(const char byte : bytes){
		stringBuilder += huffmanCodes[byte];
	}
	// 将stringBuilder转成Byte
	if(stringBuilder.length() % 8 == 0){
		len = stringBuilder.length() / 8;
	}else{
		len = stringBuilder.length() / 8 + 1;
	}
	// 创建huffmanCodeByte 存储压缩后的byte数组
	byte *huffmanCodeByte = new byte[len];
	int index = 0; // 记录第几个Byte 
	for(int i=0; i<stringBuilder.size();i+=8){
		// 每8位对应一个byte, 步长加8
		string strByte;
		if(i+8 > stringBuilder.size()){
			// 不够8位
			strByte = stringBuilder.substr(i);	 
		}else{
			strByte = stringBuilder.substr(i, 8);	
		} 
		// 将 strByte转成一个byte放入到huffmanCodeBytes
		// cout << strByte.c_str() << endl;
		huffmanCodeByte[index] = atoi(strByte.c_str());
		// cout << huffmanCodeByte[index] << endl;
		index++;
	} 
	// cout<< stringBuilder.size() << " " << stringBuilder << endl;
	len = index;
	return huffmanCodeByte;
}

// 使用一个方法，对前面的方法进行封装
byte* huffmanZip(string bytes, int &len){
	// len返回压缩后的长度
	vector<Node*> nodes = getNodes(bytes);
	// 创建霍夫曼树
	Node *root =  createHuffmanTree(nodes);
	// 根据霍夫曼树创建对应的霍夫曼编码
	getCodes(root);
	// 根据霍夫曼编码进行压缩
	byte* huffmanCodeByte = zip(bytes, haffmanCode, len);
	return huffmanCodeByte;
} 

int main(){
	string content = "i like like like java do you like a java";
	int len = 0;
	byte *huffmanCodeByte = huffmanZip(content, len);
	cout << "压缩后的结果" << endl; 
	for(int i=0; i<len; i++){
		cout << *(huffmanCodeByte+i) << " " << endl;  
	}
	cout << "压缩后长度" <<  len << endl;
	delete []huffmanCodeByte;
	huffmanCodeByte = NULL;
	system("pause");
	return 0;
}
```

