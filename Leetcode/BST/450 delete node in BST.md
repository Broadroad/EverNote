# 题目
给定一个BST和一个节点，删除这个节点，返回root
# 思路
分开讨论，有几种情况：
- 删除的节点是叶子节点，直接remove
- 删除的节点只有一个子节点，用子节点替换删除节点
- 删除的节点有两个子节点，用他的中序遍历的前驱或者后继节点替换该节点
需要注意的是用子节点或者后继或者前驱节点替换的时候实际上也是删除操作，所以也是一个递归操作。O(H)/O(H)
# 代码 (递归用后继或者前驱节点的值来替换target节点，然后删除前驱或者后继节点， 注意的是返回值是节点的指针，需要把其和root连接起来)
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root) return root;
        if (key > root->val) root->right = deleteNode(root->right, key);
        else if (key < root->val) root->left = deleteNode(root->left, key);
        else {
            // is leaf
            if (!root->left && !root->right) {
                delete root;
                root = NULL;
            } else if (root->right) {
                root->val = successor(root);
                root->right = deleteNode(root->right, root->val);             
            } else {
                root->val = predecessor(root);
                root->left = deleteNode(root->left, root->val);
            }
        }
        return root;
    }
private:
    int successor(TreeNode* root) {
        root = root->right;
        while (root->left) root = root->left;
        return root->val;
    }
    
    int predecessor(TreeNode* root) {
        root = root->left;
        while (root->right) root = root->right;
        return root->val;
    }
};
```
# 代码（和思路里的一样）
```cpp
 TreeNode* deleteNode(TreeNode* root, int val) {
        if (!root) return root;
        
        if (val < root->val) {
            root->left = deleteNode(root->left, val);
        } else if (val > root->val) {
            root->right = deleteNode(root->right, val);
        } else {
            /* Leaf node case */
            if (!root->left && !root->right) {
                delete(root);
                return NULL;
            }
            /* 1 child case */
            if (!root->left || !root->right) {
                TreeNode *ret = root->left ? root->left : root->right;
                delete(root);
                return ret;
            }
            /* 2 child case */
            if (root->left && root->right) {
                TreeNode *tmp = root->right;
                while (tmp->left) {
                    tmp = tmp->left;
                }
                root->val = tmp->val;
                root->right = deleteNode(root->right, root->val);
            }
        }
        return root;
    }
```

# 代码（删除不仅是BST，二叉树也可以）
```cpp
TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root) return nullptr;
        if (root->val == key) {
            if (!root->right) {
                TreeNode* left = root->left;
                delete root;
                return left;
            }
            else {
                TreeNode* right = root->right;
                while (right->left)
                    right = right->left;
                swap(root->val, right->val);    
            }
        }
        root->left = deleteNode(root->left, key);
        root->right = deleteNode(root->right, key);
        return root;
    }
```