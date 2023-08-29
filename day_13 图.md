### 图

```c++
// 图
/*
	为什么要有图？
	1.线性表局限于一个直接前驱和一个直接后继的关系
	2.树也只能有一个直接前驱也就是父结点
	3.需要表示多对多的关系 
*/ 

// 图是一种数据结构，其中结点可以具有零个或多个相邻元素。
// 两个结点之间的连接称为边，结点也可以称为顶点。
 /*
 	常用概念:
	 1.顶点
	 2.边
	 3.路径
	 4.无向图 
	 5.有向图
	 6.带权图 
 */ 
 
 /*
 图的表示方式
 1.邻接矩阵(二维数组) 
 	表示图形中顶点之间相邻关系的矩阵 
 2.邻接表(链表表示)
 	邻接矩阵需要为每个点都分配n个边的空间，其实由很多边都是不存在，会造成空间的一定损失。
	邻接表的实现只关心存在的边，不关心不存在的边。因此没有空间浪费，邻接表由数组+链表组成。 
 */ 
 
 # include <iostream>
 # include <vector>
 # include <string>
 using namespace std;
 /*
 思路
 1.使用string数组存储顶点
 2.保存矩阵 vector<vector<int>>edges 
 */
 inline ostream&
 operator << (ostream &os, vector<vector<int>> arrs){
 	for(vector<int> arr : arrs){
 		for (int num : arr){
 			cout << num << " ";
		 }
		 cout << endl;
	 }
 }
 class Graph{
 	private:
 		vector<string> vertexList; // 存储顶点集合
		vector<vector<int>> edges; // 存储图对应的邻接矩阵 
 		int numOfEdges; // 边的数目
	public:
		// 构造函数 
		Graph(int n):edges(vector<vector<int>>(n, vector<int>(n, 0))), numOfEdges(0){}
		// 插入结点
		void insertVertex(string vertex){
			vertexList.push_back(vertex);
		}
		// 添加边
		void insertEdge(int v1, int v2, int weight){
			// v1表示点的下标即第几个顶点 "A"-"B" "A"0 "B"1 
			// v2表示第二个点对应的下标
			// weight边的权值 
			edges[v1][v2] = weight; 
			edges[v2][v1] = weight; 
			numOfEdges++;
		}
		// 图中常用的方法
		// 返回结点的个数
		int getNumOfVertex(){
			return vertexList.size();
		}
		// 得到边的数目
		int getNumOfEdges(){
			return numOfEdges;
		} 
		// 返回v1和v2的权值
		int getWeight(int v1, int v2){
			return edges[v1][v2];
		} 
		// 返回结点i对应的数据
		string getValueByIndex(int i){
			return vertexList[i];
		}
		// 显示图对应的矩阵
		void showGraph(){
			cout << edges << endl;
		} 
		~Graph(){
		
		} 
 };	
 
 
 int main(){
 	int n = 5;
 	vector<string>VertexValue = {"A", "B", "C", "D", "E"};
 	// 创建图对象
	 Graph graph(n);
	 // 循环添加顶点
	 for (string vertex : VertexValue){
	 	graph.insertVertex(vertex); 
	 }
	 // 添加边
	 // A-B A-C B-C B-D B-E
	 graph.insertEdge(0, 1, 1);
	 graph.insertEdge(0, 2, 1);
	 graph.insertEdge(1, 2, 1); 
	 graph.insertEdge(1, 3, 1); 
	 graph.insertEdge(1, 4, 1);
	 // 显示邻接矩阵 
	 graph.showGraph(); 
 	system("pause");
 	return 0;
 } 
```

### 深度优先

```c++
 /*
 	图的深度优先遍历(DFS):
	 1.深度优先遍历，从初始访问结点出发，初始访问结点可能有多个邻接结点。
	 2.深度优先遍历策略首先访问第一个邻接结点，然后再以这个被访问的邻接结点作为初始结点，访问它的第一个邻接结点。
	 3.每次都在访问完当前结点后首先访问当前结点的第一个邻接结点。优先纵向挖掘深入。 
	 4.显然，这是一个递归的过程。 
	 
	 深度优先遍历算法步骤:
	 1.访问初始结点v，并标记结点v已被访问。
	 2.查找结点v的第一个邻接结点w。
	 3.若w存在，则继续执行4。若w不存在，则回到第一步，将从v的下一个结点继续。
	 4.若w未被访问，对w进行深度优先遍历递归(即把w当做另一个v,然后进行步骤123)。
	 5.若w被访问过，查找结点v的w邻接结点的下一个邻接结点，转到步骤3。 
 */

		// 得到第一个邻接结点的下标
		// 如果存在就返回对应的下标 否则返回-1 
		// 定义bool数组 记录某个结点是否被访问过
		vector<bool>isVisted; 
		int gerFirstNeighbor(int index){
			for(int j=0;j<vertexList.size(); ++j){
				if(edges[index][j] > 0){
					return j;
				}
			}
			return -1;
		}
		// 根据前一个邻接结点的下标来获取下一个邻接结点
		int getNextNeighbor(int v1, int v2){
			for(int j = v2 + 1; j < vertexList.size(); ++j){
				if(edges[v1][j] > 0){
					return j;
				}
			}
			return -1;
		} 
		// 深度优先遍历算法
		// i第一次就是0 
		void dfs(vector<bool> &isVisted, int i){
			// 首先访问第一个结点
			cout << this->getValueByIndex(i) + "->" << " "; 
			// 将结点设置为以访问
			isVisted[i] = true;
			// 查找该结点的第一个邻接结点
			int w = this->gerFirstNeighbor(i); 
			while(w != -1){
				// 如果存在
				if(!isVisted[w]){
					dfs(isVisted, w);
				}else{
				// 如果w被访问过
				w = this->getNextNeighbor(i, w); 					
				}
			}
		} 
		// dfs进行一个重载
		// 遍历所有的结点并进行dfs
		void dfs(){
			// 遍历所有的结点 进行dfs
			for(int i=0; i<getNumOfVertex();++i){
				if(!isVisted[i]){
					dfs(isVisted, i);
				}	
			}
		}
```



### 广度优先

```c++
		// 广度优先遍历算法
		void bfs(vector<bool> &isVisted, int i){
			int u; // 表示队列的头结点的下标
			int w; // 邻接结点下标w
			queue<int> que;
			cout << this->getValueByIndex(i) + "=>";
			// 标记为以访问
			this->isVisted[i] = true;
			// 结点加入队列
			que.push(i);
			while(!que.empty()){
				// 取出队列的头结点下标
				u = que.front();
				que.pop();
				// 得到第一个邻接结点的下标w 
				w = this->getFirstNeighbor(i);
				while(w != -1){
					if(!isVisted[w]){ 
						cout << this->getValueByIndex(w) + "=>";						
						// 入队列
						que.push(w); 
						// 标记访问过 
						isVisted[w] = true; 
					}
					// 以u为前驱结点 找w后面的下一个邻接点
					w = this->getNextNeighbor(u, w); // 体现出广度优先 
				} 
			}			 
		} 
		// 遍历所有的结点，都进行广度优先
		void bfs(){
			int n = getNumOfVertex();
			isVisted = vector<bool>(n);
			for (int i = 0; i < n; ++i){
				if(!isVisted[i]){
					bfs(isVisted, i);
				}
			}
		}
```

