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

1、迭代法  
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

2、递归法  
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




