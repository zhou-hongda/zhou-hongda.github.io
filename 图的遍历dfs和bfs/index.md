# 图的遍历：dfs和bfs


<!--more-->

### 图的遍历：dfs和bfs

#### dfs深度优先遍历：按照某一策略选择一条道路遍历图，当到达无法前进的节点时退回上一节点选择另一条道路，并以此反复递归，直到遍历所有节点。

以树（一种特殊的图）的遍历为例介绍dfs：前序，中序，后序遍历实质均为深度优先遍历

```c++
#include <stack>
#include <queue>
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode()
     : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};
//(递归，前序遍历)
void dfs(TreeNode* root){
        if(root!=nullptr)return ;
        else{
            process(root);//遍历根节点
            dfs(root->left);
            dfs(root->right);
        }
};
//(迭代)使用栈进行维护，将要遍历的节点压栈，然后出栈后检查此节点下是否还有未遍历的节点，有的话压栈，没有的话不断回溯（出栈），根据前序遍历的顺序先压右节点再压左节点
void dfs2(TreeNode* root){
    std::stack<TreeNode*> s;
    s.push(root);//压栈
    while(!s.empty()){//每次弹出结点即遍历此节点，遍历完毕，栈为空，结束循环
        TreeNode* node=s.top();//纪录节点
        s.pop();//弹出节点
        process(node);//遍历结点
        if (node->right)s.push(node->right);//压入右节点
        if (node->left)s.push(node->left);//压入左节点
    }
}
```

#### bfs广度优先遍历：指的是从图的一个未遍历的节点出发，先遍历这个节点的相邻节点，遍历完后再依次遍历每个相邻节点的相邻节点。

以树（一种特殊的图）的遍历为例介绍bfs：

```c++
//使用队列，由根节点开始进入循环，遍历该节点，出队列，判断有无子节点，有则入队列（注意这也保证了下一层遍历顺序）
void bfs(TreeNode* root){
    std::queue<TreeNode*>q;
    q.push(root);
    while (!q.empty())
    {
        TreeNode* front=q.front();
        q.pop();
        process(front);
        if(front->left) q.push(front->left);
        if(front->right) q.push(front->right);   
    }    
}
```


