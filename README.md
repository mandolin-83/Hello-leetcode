# Hello-leetcode
leetcode excercises
#leetcode 725. 分隔链表
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
        
 # leetcode 1770     
class Solution:
    def maximumScore(self, nums: List[int], multipliers: List[int]) -> int:
        n,m=len(nums),len(multipliers)
        dp=[[float('-inf')]*(m+1) for _ in range(m+1)]
        dp[0][0]=0
        res=float("-inf")
        #左边取l个，右边取r个，l+r=i,1<=i<=m
        for i in range(1,m+1):
            for j in range(i+1):  #左边取j (0<=j<=i)个,对应右边取i-j个
                l,r=j,i-j
                if l: #取左边l个
                    dp[l][r]=max(dp[l][r],dp[l-1][r]+nums[l-1]*multipliers[i-1])
                if r: #取右边r个
                    dp[l][r]=max(dp[l][r],dp[l][r-1]+nums[-r]*multipliers[i-1])
                if i==m:
                    res=max(res,dp[l][r])
        return res

