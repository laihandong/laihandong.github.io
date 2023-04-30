给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。



```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */



/*
    Failed：long long int都表示不了
    [1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1]
	[5,6,4]
	
    先把数读出来，相加后再存进链表
    时间O(n)
    空间O(n)
*/
class Solution {
public: 

    ListNode* addTwoNumbers1(ListNode* l1, ListNode* l2) {
        long long int x1 = 0, x2 = 0;
        for(int i = 0; l1 != nullptr || l2 != nullptr; i++){
            if(l1 != nullptr){
                x1 = x1 + l1->val * pow(10, i);
                l1 = l1->next;
                
            }
            if(l2 != nullptr){
                x2 = x2 + l2->val * pow(10, i);
                l2 = l2->next;
            }           
        }

        ListNode* tmpNode = new(ListNode);
        ListNode* rst = tmpNode;
        long long int y = x1 + x2;
        for(int i = 0; y != 0; i++){
            tmpNode->val = y%10;
            y = long double(y/10);
            if(y != 0){
                tmpNode->next = new(ListNode);
                tmpNode = tmpNode->next;
            }
        }

        return rst;
    }
};




/*
    对应的位相加，更新值和进位，更新遍历状态（重点）
    
    时间O(n)
    空间O(1)
    
*/
class Solution {
public: 
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int carryNumber = 0, tempL1Val = 0, tempL2Val = 0;
        ListNode *out1 = l1, *out2 = l2;
        bool rst[2] = {false, false};//代表对应的链表是否遍历完
        while(true){
            //取值
            if(!rst[0]) tempL1Val = l1->val; else tempL1Val = 0;
            if(!rst[1]) tempL2Val = l2->val; else tempL2Val = 0;
            //计算
            int tempSum = tempL1Val + tempL2Val + carryNumber;
            //更新节点的值
            carryNumber = tempSum / 10;
            if(!rst[0]) l1->val = tempSum % 10; 
            if(!rst[1]) l2->val = tempSum % 10; 
            //更新遍历的状态（重点）
            if(l1->next != nullptr) l1 = l1->next; else rst[0] = true;
            if(l2->next != nullptr) l2 = l2->next; else rst[1] = true;
            //是否跳出循环
            if(rst[0]&&rst[1]) break;
        }
        ListNode* tempNode = new(ListNode);
        if(carryNumber != 0) tempNode->val = carryNumber; else tempNode = nullptr;
        if(tempL1Val != 0){
            l1->next = tempNode;
            return out1;
        }
        else{
            l2->next = tempNode;
            return out2;
        }
    }
};
```