# LeetCode-python
我要刷题冲冲冲

## 1.两数之和
> 给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 和为目标值 `target`  的那 两个 整数，并返回它们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
你可以按任意顺序返回答案。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/two-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        for i_index,i in enumerate(nums[:-1]):
            for j_index,j in enumerate(nums[i_index+1:],start=i_index+1):
                if i+j == target:
                    return [i_index,j_index]
```

## 2.两数相加
> 给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。
请你将两个数相加，并以相同形式返回一个表示和的链表。
你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/add-two-numbers
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
+ 法一 ~~这种写法很烂~~
    ```
    # Definition for singly-linked list.
    # class ListNode:
    #     def __init__(self, val=0, next=None):
    #         self.val = val
    #         self.next = next
    class Solution:
        def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
            # 最特殊的情况是不一样长且存在进位
            result = [] # 存结果
            up = 0 # 进位符
            while l1!=None and l2!=None:
                temp = l1.val + l2.val
                if temp >= 10:
                    result.append(ListNode((temp + up) % 10, None))
                    up = 1
                else:
                    if temp == 9 and up == 1:
                        result.append(ListNode(0,None))
                        up = 1
                    else:
                        result.append(ListNode(temp + up,None))
                        up = 0
                l1 = l1.next
                l2 = l2.next
            
            # 假如两个list一样长
            if l1==None and l2==None:
                if up == 1:
                    result.append(ListNode(1,None))
            elif l1==None: # l2更长
                flag = 1
                temp = up + l2.val
                if temp >= 10:
                    while l2 != None:
                        temp = up + l2.val
                        l2.val = temp % 10
                        if flag == 1:
                            flag = 0
                            result.append(l2)
                        if temp >= 10:
                            up = 1
                        else:
                            up = 0
                        if l2.next == None and up == 1:
                            l2.next = ListNode(1,None)
                            break
                        else:
                            l2 = l2.next
                else: # 后续不用进位
                    l2.val = temp
                    result.append(l2)
            else: # l1更长
                flag = 1
                temp = up + l1.val
                if temp >= 10:
                    while l1 != None:
                        temp = up + l1.val
                        l1.val = temp % 10
                        if flag == 1:
                            flag = 0
                            result.append(l1)
                        if temp >= 10:
                            up = 1
                        else:
                            up = 0
                        if l1.next == None and up == 1:
                            l1.next = ListNode(1,None)
                            break
                        else:
                            l1 = l1.next
                else: # 后续不用进位
                    l1.val = temp
                    result.append(l1)


            # 连接ListNode
            for index in range(len(result)-1):
                result[index].next = result[index + 1]
            return result[0]

    ```
+ 法二
    ```
    # Definition for singly-linked list.
    # class ListNode:
    #     def __init__(self, val=0, next=None):
    #         self.val = val
    #         self.next = next
    class Solution:
        def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
            # 最特殊的情况是不一样长且存在进位
            root = ListNode(0, None)
            current = root
            up = 0 # 进位符
            while l1 != None or l2 != None or up != 0:
                l1_value = l1.val if l1 != None else 0
                l2_value = l2.val if l2 != None else 0
                sum = l1_value + l2_value + up
                up = int(sum / 10)
                temp = ListNode(sum % 10, None)
                current.next = temp
                current = temp

                if l1 != None:
                    l1 = l1.next
                if l2 != None:
                    l2 = l2.next
            
            return root.next
    ```