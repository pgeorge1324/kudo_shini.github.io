# dfs&bfs总结

- ## 经典dfs

  ```c++
  void dfs(TreeNode* node){
  	if(!node) return;
      dfs(node->left);
      cout<<node->val<<endl;
      dfs(node->right);
  } //dfs+中序遍历
  ```

- ## 经典bfs

  ```c++
  void bfs(TreeNode* root){
  	if (!root) return;
      queue<TreeNode *> Q;
      Q.push(root);
      while (!Q.empty()) {
          int size = Q.size();
          while (size > 0) {
              TreeNode* node = Q.front();
              Q.pop();
              cout<<Q->val<<endl;
              if (node->left) Q.push(node->left);
              if (node->right) Q.push(node->right);
              size--;
          }
      }
  }
  ```

- ## 两树比较，树的对称性

  ```c++
  // 两树比较
  // dfs
  bool isSameTree(TreeNode* p, TreeNode* q) {
      if (!p&&!q) return true;
      if (!p||!q) return false;
      return p->val==q->val&&isSameTree(p->left,q->left)&&isSameTree(p->right,q->right);
  }
  // bfs
  bool isSameTree(TreeNode* p, TreeNode* q) {
  
      queue<TreeNode*> tree;
      tree.push(p);
      tree.push(q);
      while (!tree.empty()){
          p = tree.front(); tree.pop();
          q = tree.front(); tree.pop();
          if (!p&&!q) continue;
          if (!p||!q||p->val!=q->val) return false;
          tree.push(p->left);
          tree.push(q->left);
          tree.push(p->right);
          tree.push(q->right);
      }
      return true;
  }
  // 树的对称性 交换插入顺序即可
  // dfs
  bool recurse(TreeNode *left, TreeNode *right) {
  
      if (!left && !right) return true;
      if (!left || !right) return false;
      return left->val == right->val && recurse(left->left, right->right) && recurse(left->right, right->left);
  }
  // bfs
  bool check(TreeNode *left, TreeNode *right) {
  
      queue<TreeNode *> queue;
      queue.push(left);
      queue.push(right);
      while (!queue.empty()) {
          left = queue.front();
          queue.pop();
          right = queue.front();
          queue.pop();
          if (left == right) continue;
          if (!left || !right || left->val != right->val) return false;
          queue.push(left->left);
          queue.push(right->right);
          queue.push(left->right);
          queue.push(right->left);
      }
      return true;
  }
  bool isSymmetric(TreeNode *root) {
  
      if (!root) return true;
      return check(root->left, root->right);
  
  }
  ```

- ## 深度问题

  - 最大深度

    ```c++
    // dfs
    int maxDepth(TreeNode *root) {
        if (root == nullptr) return 0;
        return std::max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
    // bfs
    // 每多一层ans++
    int maxDepth(TreeNode* root) {
            if (root == nullptr) return 0;
            queue<TreeNode*> Q;
            Q.push(root);
            int ans = 0;
            while (!Q.empty()) {
                int sz = Q.size();
                while (sz > 0) {
                    TreeNode* node = Q.front();Q.pop();
                    if (node->left) Q.push(node->left);
                    if (node->right) Q.push(node->right);
                    sz -= 1;
                }
                ans += 1;
            } 
            return ans;
    }
    ```

  - 叶子节点的最小深度

    ```c++
    // dfs
    // 当节点是叶子节点时开始返回有效值，比较左右子树最小深度
    int minDepth(TreeNode *root) {
    
        if (!root) return 0;
        int l = minDepth(root->left);
        int r = minDepth(root->right);
        if (l == 0 && r == 0) {
            return 1;
        } else if (l == 0) {
            return r + 1;
        } else if (r == 0) {
            return l + 1;
        } else {
            return std::min(l, r) + 1;
        }
    
    }
    
    // bfs
    // 层次遍历到第一个叶子节点时返回
    int minDepth(TreeNode *root) {
    
        if (!root) return 0;
        queue<TreeNode *> Q;
        Q.push(root);
        int count = 0;
        while (!Q.empty()) {
            int size = Q.size();
            count++;
            while (size > 0) {
                TreeNode *node = Q.front();
                Q.pop();
                if (!node->left && !node->right) return count;
                if (node->left) Q.push(node->left);
                if (node->right) Q.push(node->right);
                size--;
            }
        }
        return count;
    
    }
    ```

- ## 二叉搜索树的处理

  - 顺序处理两个节点之间的关系

    ```c++
    // leetcode530
    // 使用pre存储前一个节点，pre=-1处理第一个节点
    void getMinimumDifferenceDfs(TreeNode *node, int &pre, int &ans) {
    
        if (!node) return;
        if (node->left) getMinimumDifferenceDfs(node->left, pre, ans);
        if (pre == -1) {
            pre = node->val;
        } else {
            ans = std::min(ans, node->val - pre);
        }
        if (node->right)getMinimumDifferenceDfs(node->right, pre, ans);
    
    }
    
    int getMinimumDifference(TreeNode *root) {
    
        int ans = INT_MAX;
        int pre = -1;
        getMinimumDifferenceDfs(root, pre, ans);
        return ans;
    
    }
    ```

  - 在顺序遍历的同时保持return头节点

    ```c++
    // leetcode897题
    TreeNode *increasingBST(TreeNode *root, TreeNode *tail = nullptr) {
        if (!root) return tail;
        auto head = increasingBST(root->left, root);
        root->left = nullptr;
        root->right = increasingBST(root->right, tail);
        return head;
    }
    ```
    
  - 找寻两个节点的最深共同祖先

    ```
    // leetcode235
    // 从根出发，如果当前值大于p,小于q那它一定是最小祖先
    // 如果都大于当前值，去右子树，否则相反
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
            
            if(!root) return nullptr;
            if(root->val < p->val&& root->val < q->val){
                return lowestCommonAncestor( root->right, p,  q);
            }else if(root->val > p->val&& root->val > q->val){
                return lowestCommonAncestor( root->left, p,  q);
            }else{
                return root;
            }
    }
    ```

    

- ## 根据有序节点建树

  ```c++
  // (a+b)/2处理越界时使用移位
  TreeNode *buildTree(int s, int e, vector<int> &nums) {
  
      if (s > e) return nullptr;
      int half = (s >> 1) + (e >> 1) + ((s | e) & 1);;
      TreeNode *newNode = new TreeNode(nums[half]);
      newNode->left = buildTree(s, half-1, nums);
      newNode->right = buildTree(half + 1, e, nums);
      return newNode;
  }
  
  TreeNode *sortedArrayToBST(vector<int> &nums) {
      return buildTree(0, nums.size()-1, nums);
  }
  ```

  