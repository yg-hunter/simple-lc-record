# [leetcode resolutions](#leetcode-resolutions)
<br/>
<br/>

<h1 id="0">TABEL OF CONTENTS</h1>  

- ## linked list
    - [206.  反转链表](#1.1)
    
    
    
    <br/>  
    <br/>  
    
    
    
 
<h1 id="1.1"> LeetCode 206 </h1>  [回到目录](#0)  
## 1 [反转链表 reverse linked list](https://leetcode-cn.com/problems/reverse-linked-list/)

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
