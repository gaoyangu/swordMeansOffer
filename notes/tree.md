## 二叉树

## 68. 树中两个节点的最近公共祖先

### 类型1：二叉搜索树

- [牛客网: JZ68 二叉搜索树的最近公共祖先](https://www.nowcoder.com/practice/d9820119321945f588ed6a26f0a6991f)

**二叉搜索树：**

1. 排序
2. 位于左子树的节点都比父节点小
3. 位于右子树的节点都比父节点大

**思路：**

1. 从树的根节点开始和两个输入的节点进行比较
2. 如果当前节点的值比两个节点的值都大，则最近公共祖先一定在当前节点的左子树
3. 如果当前节点的值比两个节点的值都小，则最近公共祖先一定在当前节点的右子树

```cpp
int lowestCommonAncestor(TreeNode* root, int o1, int o2) {
    if(root == nullptr){
        return -1;
    }
    return process(root, o1, o2)->val;
}
TreeNode* process(TreeNode* root, int o1, int o2){
    if(root == nullptr || root->val == o1 || root->val == o2){
        return root;
    }
    if(o1 < root->val && o2 < root->val){
        return process(root->left, o1, o2);
    }else if(o1 > root->val && o2 > root->val){
        return process(root->right, o1, o2);
    }
    return root;
}
```

### 类型2：有指向父节点指针的二叉树

问题可转换为：求两个链表的第一个公共节点
```cpp
// todo
```

### 类型3：普通的二叉树

非递归：

- [leetcode: 剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(root == nullptr){
        return nullptr;
    }
    unordered_map<TreeNode*, TreeNode*> fatherMap;
    fatherMap[root] = root;
    dfs(root, fatherMap);
    
    unordered_set<TreeNode*> s;
    TreeNode* cur = p;
    while(cur != fatherMap[cur]){
        s.insert(cur);
        cur = fatherMap[cur];
    }
    cur = q;
    while(cur != fatherMap[cur]){
        if(s.find(cur) != s.end()){
            return cur;
        }
        cur = fatherMap[cur];
    }
    return root;
}
void dfs(TreeNode* root, unordered_map<TreeNode*, TreeNode*>& fatherMap){
    if(root == nullptr){
        return;
    }
    if(root->left != nullptr){
        fatherMap[root->left] = root;
        dfs(root->left, fatherMap);
    }
    if(root->right != nullptr){
        fatherMap[root->right] = root;
        dfs(root->right, fatherMap);
    }
}
```

递归：

1. 其中一个节点是另一个节点的最近公共祖先
2. 两个节点不互为最近公共祖先

- [牛客网: JZ86 在二叉树中找到两个节点的最近公共祖先](https://www.nowcoder.com/practice/e0cc33a83afe4530bcec46eba3325116)

```cpp
int lowestCommonAncestor(TreeNode* root, int o1, int o2) {
    if(root == nullptr){
        return -1;
    }
    return process(root, o1, o2)->val;
}
TreeNode* process(TreeNode* root, int o1, int o2){
    if(root == nullptr || root->val == o1 || root->val == o2){
        return root;
    }
    TreeNode* left = process(root->left, o1, o2);
    TreeNode* right = process(root->right, o1, o2);
    if(left == nullptr){
        return right;
    }
    if(right == nullptr){
        return left;
    }
    return root;
}
```

