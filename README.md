# [Leetcode Resolutions](#leetcode-resolutions)
<br/>
<br/>

<h1 id="0">TABEL OF CONTENTS</h1>  

- ## [Linked List](#1)
    - [206.  反转链表](#1.1)
    - [21.  合并两个有序链表](#1.2)
    - [19.  删除链表的倒数第N个节点](#1.3)
    - [24. 两两交换链表中的节点](#1.4)  
    - [61. 旋转链表](#1.5)  
    - [44. 分隔链表](#1.6)   
    - [141. 环形链表](#1.7)   
    - [142. 环形链表 Ⅱ](#1.8)  
    

- ## [Binary Tree](#2)
    - [94. 二叉树的中序遍历](#2.1)  
    - [144. 二叉树的前序遍历](#2.2)    
    - [145. 二叉树的后序遍历](#2.3)    
    
    
    <br/>    
    <br/>   
    
 <h1 id="1"> Linked List </h1>  [回到目录](#0)   
   <br/>  
   <br/>   
   
   

     
<h1 id="1.1"> LeetCode 206 </h1>  [回到目录](#0)   
## 1 [反转链表 reverse linked list](https://leetcode-cn.com/problems/reverse-linked-list/)  

### 1.1 迭代法  
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr || head->next == nullptr)
            return head;
        
        ListNode *pre = head; // head结点就是头结点，也存数据，所以要从head开始处理
        ListNode *cur = pre->next;
        pre->next = nullptr; // 头结点变为尾结点后，后继要置空

        ListNode *tmp = nullptr;
        while(cur!=nullptr) {
            tmp = cur->next; // 暂存,防断链后丢失后面结点
            
            cur->next = pre; // 反转

            // 后移一个
            pre = cur; 
            cur = tmp;
        }

        return pre;
    }
};
```   

更精炼的写法可以这样：  
```c++
ListNode* reverseList(ListNode* head) 
{        
    ListNode *pre = nullptr;
    ListNode *cur = head;

    ListNode *tmp = nullptr;
    while(cur!=nullptr) {
        tmp = cur->next; // 暂存,防断链后丢失后面结点

        cur->next = pre; // 反转

        // 后移一个
        pre = cur; 
        cur = tmp;
    }

    return pre;
}
```

### 1.2 递归法  
```c++
ListNode* reverseList(ListNode* head) 
{        
    if (head == nullptr || head->next == nullptr) 
        return head;

    ListNode* pre = reverseList(head->next);
    head->next->next = head;
    head->next = nullptr;
    return pre;
}
```

最后，我发现我把上面代码改写成c代码，耗时跟空间占用都减少了不少（包括递归解法），比如方法2：
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* reverseList(struct ListNode* head){        
        struct ListNode *pre = NULL; // head结点就是头结点，也存数据，所以要从head开始处理
        struct ListNode *cur = head;

        struct ListNode *tmp = NULL;
        while(cur!=NULL) {
            tmp = cur->next; // 暂存,防断链后丢失后面结点
            
            cur->next = pre; // 反转

            // 后移一个
            pre = cur; 
            cur = tmp;
        }

        return pre;
}
```    
   
   
   
   <br/>    
   <br/>    

   
     
<h1 id="1.2"> LeetCode 21 </h1>  [回到目录](#0)  
## 2 [合并两个有序链表 merge two sorted lists](https://leetcode-cn.com/problems/merge-two-sorted-lists/)  

### 2.1 迭代法  

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2){
    if(l1 == NULL)
        return l2;
    if(l2 == NULL)
        return l1;
    
    struct ListNode *cur1 = l1, *cur2 = l2;
    struct ListNode *l3 = NULL; // 合并后的链表头结点
    if(l1->val < l2->val){
        l3 = l1;
        cur1 = cur1->next; // 后移一个，防止后面循环中头结点的地方形成环
    }
    else {
        l3 = l2;
        cur2 = cur2->next;
    }

    struct ListNode *l3_next = l3;
    
    // 逐个遍历两个链表的数据，较小者并入l3
    while(cur1!=NULL && cur2!=NULL) {
        if(cur1->val <= cur2->val) {
            l3_next->next = cur1;
            cur1 = cur1->next;
        }
        else {
            l3_next->next = cur2;
            cur2 = cur2->next;
        }
        l3_next = l3_next->next;
    }
    l3_next->next = (cur1==NULL)?cur2:cur1; // 剩余节点，拼接到l3链表尾部即可

    return l3;
}
```

上面代码改成c++后，耗时及内存消耗都增加了不少，如下表:    
  

提交时间 | 提交结果 |  执行用时 | 内存消耗 | 语言
-|-|-|-|-|
几秒前 | 通过 | 12 ms | 7.1 MB | Cpp |
2 分钟前 | 通过 | 4 ms | 5.7 MB | C |  
  
  

上面代码可以再优化，用一个临时的对象，作为新合并后链表的头，这样写法上就更简洁了，如下：
```C
struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2){
    struct ListNode obj_head = {0}; // 存储合并后的链表的头，要初始化为空，因为如果l1、l2为空时，新链表也要为空
    struct ListNode *l3_next = &obj_head;
    
    while(l1!=NULL && l2!=NULL) {
        if(l1->val <= l2->val) {
            l3_next->next = l1;
            l1 = l1->next;
        }
        else {
            l3_next->next = l2;
            l2 = l2->next;
        }
        l3_next = l3_next->next;
    }
    
    l3_next->next = (l1==NULL)?l2:l1; 

    return obj_head.next;
}
```

### 2.2 递归法  
```c
struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2){
    if(l1 == NULL)
        return l2;
    
    if(l2 == NULL)
        return l1;
    
    if(l1->val <= l2->val) {
        l1->next = mergeTwoLists(l1->next, l2);
        return l1;
    }
    else {
        l2->next = mergeTwoLists(l1, l2->next);
        return l2;
    }
}
```    
   
      
  
  <br/>    
  <br/>    
    
    
     
<h1 id="1.3"> LeetCode 19 </h1>  [回到目录](#0)  
## 3 [删除链表的倒数第N个节点 remove nth node from end of list](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)  

### 3.1 迭代法  
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

struct ListNode* removeNthFromEnd(struct ListNode* head, int n){
    if(head==NULL)
        return head;

    struct ListNode *pre_nth_node = head;
    struct ListNode *fast_node = head;

    for(int i = 0; i < n && fast_node!=NULL; i++)
        fast_node = fast_node->next;
        
    if(fast_node==NULL){ // only one or two nodes, delete the last one
        head = head->next;
        return head;
    }
    
    while(fast_node!=NULL && fast_node->next!=NULL){
        pre_nth_node = pre_nth_node->next;
        fast_node = fast_node->next;
    }
    if(pre_nth_node->next==NULL) // only two nodes, delete the first one
        head->next = NULL;
    else
        pre_nth_node->next = pre_nth_node->next->next;
    
    return head;
}
```  
双百的代码，执行结果：   
  
  
提交时间 | 提交结果 |  执行用时 | 内存消耗 | 语言
-|-|-|-|-|
几秒前 | 通过 | 0 ms | 5.3 MB | C |    
  
   
   

  
  <br/>    
  <br/>    

     
<h1 id="1.4"> LeetCode 24 </h1>  [回到目录](#0)   
## 4 [两两交换链表中的节点 swap nodes in pairs](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)    

### 4.1 迭代法  
需要三个指针：要交换的两个元素指针，及其前驱指针。  
构造一个头指针，可以用一个临时变量指向head，最后返回头指针的next即可。  
```c
struct ListNode* swapPairs(struct ListNode* head){
    if(head == NULL || head->next == NULL)
        return head;
    
    struct ListNode obj_head;
    obj_head.next = head;
    struct ListNode *prev = &obj_head;
    struct ListNode *curr1 = prev->next;
    struct ListNode *curr2 = curr1->next, *tmp = NULL;

    while(curr2 != NULL) {
        tmp = curr2->next;

        curr2->next = curr1;
        curr1->next = tmp;
        prev->next = curr2;
        
        prev = curr1;
        curr1 = curr1->next;
        if(curr1 == NULL)
            break;
        
        curr2 = curr1->next;
    }

    return obj_head.next;
}
```   

  
  
### 4.2 递归
```c
struct ListNode* swapPairs(struct ListNode* head){
    if (head == NULL || head->next == NULL)
        return head;

    struct ListNode *curr1 = head;
    struct ListNode *curr2 = curr1->next;

    curr1->next  = swapPairs(curr2->next);
    curr2->next = curr1;

    return curr2;
}
 ```       
   
   
      
   

   
   
  
  <br/>    
  <br/>    

     
<h1 id="1.5"> LeetCode 61 </h1>  [回到目录](#0)    
## 5 [旋转链表 rotate list](https://leetcode-cn.com/problems/rotate-list/)     
  
  
给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。  
```
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
```

### 5.1 迭代法   

```c
struct ListNode* rotateRight(struct ListNode* head, int k){
    if(k <= 0 || head == NULL)
        return head;
    
    struct ListNode *slow = head, *fast = head;
    for(int i = 0; i < k; i++) {
        fast= fast->next;
        if(fast == NULL) {
            fast = head;
            k = k%(i+1) + i+1; //i+1 is nodes counts
        }
    }
    if(fast == slow) // no need op
        return head;

    while(fast->next!=NULL){
        fast = fast->next;
        slow = slow->next;
    }

    struct ListNode *new_head = slow->next;
    slow->next = NULL;
    fast->next = head;

    return new_head;
}
```  
  
  <br/>     
  <br/>     
  
  

     
<h1 id="1.6"> LeetCode 44 </h1>  [回到目录](#0)    
## 6 [分隔链表 partition list](https://leetcode-cn.com/problems/partition-list/)     
给定一个链表和一个特定值 `x`，对链表进行分隔，使得所有小于 `x` 的节点都在大于或等于 `x` 的节点之前。   

**注意** 你应当保留两个分区中每个节点的初始相对位置。   

c代码如下：  
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

struct ListNode* partition(struct ListNode* head, int x){
    if(head == NULL || head->next == NULL)
        return head;

    struct ListNode obj_head; // 临时变量，指向链表头结点，因为head也有可能需要移动到链表后面
    obj_head.next = head;

    struct ListNode* curr = &obj_head;  // 指向大于x的元素的前一个节点
    struct ListNode* prev_less = curr;  // less的前驱，移动less时需要
    struct ListNode* less = curr->next; // 用于遍历指针，找到小于x的元素
    while(curr!=NULL && less!=NULL) {
        if(less->val < x) { // 找到小于x的元素
            if(curr->next!=less) { // 暂无大于x的节点
                struct ListNode* tmp = curr->next;
                curr->next = less;
                prev_less->next = less->next;
                less->next = tmp;
            }
            
            curr = curr->next;
            prev_less = less;
            less = less->next;
        }
        else {
            prev_less = less;
            less = less->next;
        }
    }
    return obj_head.next;
}
```   
相同的代码，改成c++之后，提交指向结果对比：  

提交时间 | 提交结果 |  执行用时 | 内存消耗 | 语言
-|-|-|-|-|
几秒前 | 通过 | 8 ms | 6.7 MB | Cpp |
2 分钟前 | 通过 | 0 ms | 5.4 MB | C |     
  
  
  

    
  
  <br/>    
  <br/>    
    
    
     
<h1 id="1.7"> LeetCode 141 </h1>  [回到目录](#0)  
## 7 [环形链表 linked list cycle](https://leetcode-cn.com/problems/linked-list-cycle/)  

### 7.1 快慢指针  
c++代码如下：
```c++
bool hasCycle(ListNode *head) 
{
    if(head == nullptr)
        return false;

    ListNode *slow = head, *fast = head->next;
    while(fast != nullptr) {
        if(slow == fast)
            return true;

        slow = slow->next;
        if(fast->next == nullptr)
            return false;

        fast = fast->next->next;
    }
    return false;
}
```  

### 7.2 缓存map/set中，查找法
```c++
bool hasCycle(ListNode *head) 
{
    if(head == nullptr)
        return false;

    map<ListNode*, int> m_node;
    while(head!=NULL) {
        //map<ListNode*, int>::iterator it = m_node.find(head);
        auto it = m_node.find(head);
        if(it == m_node.end()) { // not found           
            //m_node.insert(map<ListNode*, int>::value_type(head, head->val));
            m_node.insert(pair<ListNode*, int>(head, head->val));
            head = head->next;
        }
        else { // find it
            return true;
        }
    }
    return false;
}
```    
  
  
  
  <br/>    
  <br/>    
    
    
     
<h1 id="1.8"> LeetCode 142 </h1>  [回到目录](#0)  
## 8 [环形链表 Ⅱ linked list cycle ii](https://leetcode-cn.com/problems/linked-list-cycle-ii/)  

### 8.1 缓存map/set中，查找法  
c++代码如下：
```c++
ListNode *detectCycle(ListNode *head) 
{
    if(head == nullptr)
        return nullptr;
    
    unordered_set<ListNode*> s_node;
    ListNode* tmp_node = head;
    while(tmp_node != nullptr) {
        if(s_node.find(tmp_node) == s_node.end()) { //not found
            s_node.insert(tmp_node);
            tmp_node = tmp_node->next;
        }
        else {
            return tmp_node;
        }
    }
    return nullptr;        
}
```  

### 8.2 双指针  
```c++
ListNode *detectCycle(ListNode *head) 
{
    if(head == nullptr)
        return nullptr;
    
    ListNode* slow = head, *fast = head->next;
    while(fast != nullptr) {
        if(fast == slow) // 有环
            break;
        
        slow = slow->next;
        if(fast->next != nullptr) {
            fast = fast->next->next;
        }
        else {
            fast = nullptr; // 无环
            break;
        }
    }

    if(fast != nullptr) { // 确定有环
        ListNode* tmp_node = head;
        while(tmp_node != fast) {
            tmp_node = tmp_node->next;
            fast = fast->next;
        }
    }
    return fast;
}
```  



  <br/>  
  <br/>  
  <br/>
  
 <h1 id="2"> BINARY TREE </h1>  [回到目录](#0)   
   
   <br/>    
   <br/>    
  
<h1 id="2.1"> LeetCode 94 </h1>  [回到目录](#0)      
## 1 [二叉树的中序遍历 binary-tree inorder traversa](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)       
  
  
给定一个二叉树，返回它的中序 遍历。  

**进阶**: 递归算法很简单，你可以通过迭代算法完成吗？  

### 1.1 递归法  
c++代码如下：  
```c++
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
    void do_inorder_traversal(TreeNode* node, vector<int>& res) {
        if(node != nullptr) {
            do_inorder_traversal(node->left, res);
            res.push_back(node->val);
            do_inorder_traversal(node->right, res);
        }
    }
    
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        
        do_inorder_traversal(root, res);

        return res;        
    }
};
```  

### 1.2 迭代法  
需要借用栈来存储每颗子树的根节点  

```c++    
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;

    stack<TreeNode*> s_roots;        
    TreeNode* tmp_root = root;
    while(tmp_root!=nullptr || !s_roots.empty()) {
        while(tmp_root!=nullptr) {
            s_roots.push(tmp_root);
            tmp_root = tmp_root->left;
        }

        tmp_root = s_roots.top();
        s_roots.pop();
        res.push_back(tmp_root->val);

        tmp_root = tmp_root->right;
    }

    return res;        
}
```

  
  
  
   <br/>    
   <br/>    
  
<h1 id="2.2"> LeetCode 144 </h1>  [回到目录](#0)  
## 2 [二叉树的前序遍历 binary-tree preorder traversa](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)     
   
   
给定一个二叉树，返回它的 `前序` 遍历。  

**进阶**: 递归算法很简单，你可以通过迭代算法完成吗？  

### 2.1 递归法  
c++代码如下：  
```c++
class Solution {
public:
    void do_preorder_traversal(TreeNode* node, vector<int>& res) {
        if(node != nullptr) {
            res.push_back(node->val);
            do_preorder_traversal(node->left, res);
            do_preorder_traversal(node->right, res);
        }
    }
    
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        
        do_preorder_traversal(root, res);

        return res;        
    }
};
```    
### 2.2 迭代法    
```c++
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;

    stack<TreeNode*> s_roots;        
    TreeNode* tmp_root = root;
    while(tmp_root!=nullptr || !s_roots.empty()) {
        while(tmp_root!=nullptr) {  // 一直往下到最左孩子
            res.push_back(tmp_root->val);
            s_roots.push(tmp_root);
            tmp_root = tmp_root->left;
        }

        tmp_root = s_roots.top();
        s_roots.pop();

        tmp_root = tmp_root->right;
    }

    return res;  ;        
}
```  
  
  



  
   <br/>    
   <br/>    
  
<h1 id="2.3"> LeetCode 145 </h1>  [回到目录](#0)   
## 3 [二叉树的后序遍历 binary-tree postorder traversa](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)    
  
  
给定一个二叉树，返回它的 `后序` 遍历。  

**进阶**: 递归算法很简单，你可以通过迭代算法完成吗？  

### 3.1 递归法  
c++代码如下：  
```c++
void do_postorder_traversal(TreeNode* node, vector<int>& res) {
    if(node != nullptr) {
        do_postorder_traversal(node->left, res);
        do_postorder_traversal(node->right, res);
        res.push_back(node->val);
    }
}

vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    
    do_postorder_traversal(root, res);

    return res;        
}
```    
### 3.2 迭代法    

```c++
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> s_roots;
    TreeNode* tmp_root = root;
    TreeNode* prev = nullptr;

    while(tmp_root!=nullptr) { // 一直到最左孩子节点
        s_roots.push(tmp_root);
        tmp_root = tmp_root->left;
    }

    while(!s_roots.empty()) {
        tmp_root = s_roots.top();
        if(tmp_root->right==nullptr || tmp_root->right==prev) {
            res.push_back(tmp_root->val);
            prev = tmp_root;
            s_roots.pop();
        }
        else { // 右孩子
            tmp_root = tmp_root->right;
            while(tmp_root!=nullptr) {
                s_roots.push(tmp_root);
                tmp_root = tmp_root->left;
            }
        }
    }

    return res;        
}
```  
  
  

