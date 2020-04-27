# [leetcode resolutions](#leetcode-resolutions)
<br/>
<br/>

<h1 id="0">TABEL OF CONTENTS</h1>  

- ## linked list
    - [206.  反转链表](#1.1)
    - [21.  合并两个有序链表](#1.2)
    
    
    
    <br/>  
    <br/>  
     
<h1 id="1.1"> LeetCode 206 </h1>  [回到目录](#0)  
## 1 [反转链表 reverse linked list](https://leetcode-cn.com/problems/reverse-linked-list/)

1.1 迭代法  
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

1.2 递归法  
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
     
<h1 id="1.2"> LeetCode 206 </h1>  [回到目录](#0)  
## 2 [合并两个有序链表 merge two sorted lists](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

2.1 迭代法  

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

2.2 递归法
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



