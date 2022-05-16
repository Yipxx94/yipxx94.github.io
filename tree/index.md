# 迭代法实现二叉树的前序、中序和后续遍历


<!--more-->


## 本文的目的是为了总结出一个通用的，改动较少的非递归模板，可以分别适用于二叉树的前序、中序和后续遍历，方便记忆。

### 前序遍历
1. 前序遍历的遍历顺序为：**根 -> 左 -> 右**。
2. 只要有左子树，就把左子树入栈，同时把值加入答案数组，然后依次弹出栈顶元素，移动到它的右子树，重复操作。
```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        while (root || stk.size())
        {
            while (root)
            {
                res.push_back(root->val);
                stk.push(root);
                root = root->left;
            }
            root = stk.top();
            stk.pop();
            root = root->right;
        }
        return res;
    }
};
```

### 中序遍历
1. 中序遍历的遍历顺序为：**左 -> 右 -> 根**。
2. 只要有左子树，就把左子树入栈，然后依次弹出栈顶元素，弹出的同时时把值加入答案数组，移动到它的右子树，重复操作。
```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        while (root || stk.size())
        {
            while (root)
            {
                // res.push_back(root->val);  与前序遍历不一样的地方
                stk.push(root);
                root = root->left;
            }
            root = stk.top();
            stk.pop();
            res.push_back(root->val);    // 与前序遍历唯一不同的地方
            root = root->right;
        }
        return res;
    }
};
```

### 后序遍历
1. 后序遍历的遍历顺序为：**左 -> 右 -> 根**。
2. 已知前序遍历的遍历顺序为：**根 -> 左 -> 右**，我们可以将后续遍历的顺序反过来，变成**根 -> 右 -> 左**，这样的话遍历的方法只需在前序遍历的基础上改动一下即可。
3. 只要有右子树，就把右子树入栈，同时把值加入答案数组，然后依次弹出栈顶元素，移动到它的左子树，重复操作。
4. 最后，翻转一下答案数组即可。
```
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        while (root || stk.size())
        {
            while (root)
            {
                res.push_back(root->val);
                stk.push(root);
                root = root->right;    // root = root->left  与前序遍历不一样的地方
            }
            root = stk.top();
            stk.pop();
            root = root->left;    // root = root->right  与前序遍历不一样的地方
        }
        reverse(res.begin(), res.end());    // 翻转答案数组
        return res;
    }
};
```

