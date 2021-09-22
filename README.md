# Hello-leetcode
leetcode excercises
#725. 分隔链表
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def splitListToParts(self, head: ListNode, k: int) -> List[ListNode]:
        n=0
        cur=head
        while cur!=None:
            n+=1
            cur=cur.next
        
        q=n//k  #每组里至少有几个元素
        r=n%k   #前r组里每组多1个元素
        #print(q,r)
        ans=[None for _ in range(k)]
        #print(ans)
        cur=head
        for i in range(k):
            if cur:
                ans[i]=cur
                if i<r:
                    part_size=q+1
                else:
                    part_size=q
                j=1
                while j<part_size:
                    cur=cur.next
                    j+=1
                tem=cur.next
                cur.next=None
                cur=tem
        return ans
