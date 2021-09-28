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

leetcode 430
"""
# Definition for a Node.
class Node:
    def __init__(self, val, prev, next, child):
        self.val = val
        self.prev = prev
        self.next = next
        self.child = child
"""

class Solution:
    def flatten(self, head: 'Node') -> 'Node':
        if not head:return head
        self.l=[]
        def inorder(node,l):
            if not node:return
            l.append(node.val)
            inorder(node.child,l)
            inorder(node.next,l)
        
        inorder(head,self.l)
        new_head=Node(self.l[0],None,None,None)
        pre=new_head
        for i in range(1,len(self.l)):
            cur=Node(self.l[i],pre,None,None)
            pre.next=cur
            pre=cur
        return new_head
  # 1547
  class Solution:
    def minCost(self, n: int, cuts: List[int]) -> int:
        cuts.sort()
        new_cuts=[0]+cuts+[n]
        #n=7,cuts=[1,3],new_cuts=[0,1,3,7]
        m=len(new_cuts)
        dp=[[0]*m for _ in range(m)] 
        #n=7,cuts=[1,3],new_cuts=[0,1,3,7]
        #先三个中间切一刀(切点1，3),再四个中间切一刀（切点1，3，中间其他点前面已经切好）
        for l in range(2,m): #len=2,3
            for i in range(0,m-l): #i为左端点，i+l<m 
                j=i+l  #j为右端点
                dp[i][j]=float("inf")
                for k in range(i+1,j):#在区间[i,j]中间切一刀
                    dp[i][j]=min(dp[i][j],dp[i][k]+dp[k][j]+new_cuts[j]-new_cuts[i])
        return dp[0][m-1]


#leetcode 1745
                                                class Solution:
    def checkPartitioning(self, s: str) -> bool:
        size=len(s)
        dp=[[False]*size for _ in range(size)]
        for i in range(size): #所有长度为1的子串为回文子串
            dp[i][i]=True
        for i in range(size-1): #判断所有长度为2的子串是否为回文字串
            if s[i]==s[i+1]:
                dp[i][i+1]=True
        for l in range(2,size): #判断长度为3到size的子串是否为回文字串
            for i in range(size-l): #遍历字符串开始位置
                j=i+l
                if s[i]==s[j] and dp[i+1][j-1]==True:
                    dp[i][j]=True
        #print(dp)
        
        now={0} #记录下一轮找回文字串的首字母位置
        for _ in range(3): #一共找3轮，看是否可以找到s末尾
            tem=set()
            for i in now:
                for j in range(i,size):
                    if dp[i][j]==True:
                        tem.add(j+1) #记录下一轮找回文字串的首字母位置
            now=tem
            
        return size in now
    
  #1931
    class Solution:
    def minimizeTheDifference(self, mat: List[List[int]], target: int) -> int:
        m,n=len(mat),len(mat[0])
        f={0}
        large=float("inf") #记录大于target的最小值
        for i in range(m):
            g=set() 
#下一个大于target的值来源于两种途径：本行里的每个数加上f(0到上一行中所有小于target的和)大于target的最小值，或者（0-上一行）最小的大于target的值加上本行的数，两者取小
            next_large=float("inf")
            for x in mat[i]:
                for j in f:
                    if x+j>=target:#本行里的1个数加上f(f中的数都<target)里的所有值
                        next_large=min(next_large,x+j)
                    else:
                        g.add(x+j)
                next_large=min(next_large,large+x)#（0-上一行）最小的大于target的值加上本行的数
            f=g
            large=next_large
        
        ans=abs(target-large) #计算target-大于target的最小数
        for i in f:
            ans=min(ans,abs(i-target)) #计算target-小于target的最大数
        return ans
