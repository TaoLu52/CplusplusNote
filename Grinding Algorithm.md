## 一、 贪心算法
### 分配问题
#### [455. 分发饼干](https://leetcode.cn/problems/assign-cookies/)  (A)
主要思路，先排序，当饼干大于饥饿度，同时到下一个，当饼干小于饥饿度，则看下一块饼干
```c++
class Solution {

public:

    int findContentChildren(vector<int>& g, vector<int>& s) {

        if(s.empty()==true)

        {

            return 0;

        }

        sort(g.begin(),g.end());

        sort(s.begin(),s.end());

        int cnt=0;

        int gcnt,scnt;

        gcnt=scnt=0;

        while(gcnt<g.size() && scnt<s.size())

        {

            if(g[gcnt]<=s[scnt])

            {

                cnt++;

                gcnt++;

                scnt++;

            }

            else

            {

                scnt++;

            }

  

        }

        return cnt;

    }

};
```

#### [135. 分发糖果](https://leetcode.cn/problems/candy/) (A)
从左到右扫描一遍，之后从右到左扫描一遍。使用额外的数组存储状态
```c++
class Solution {

public:

    int candy(vector<int>& ratings) {

        if(ratings.empty())

        {

            return 0;

        }

        // sort(ratings.begin(),ratings.end());

        int cnt=0;

        vector<int> candy(ratings.size(),1);

        for(cnt=1;cnt<ratings.size();cnt++)

        {

            if(ratings[cnt]>ratings[cnt-1])

                candy[cnt]=candy[cnt-1]+1;

        }

        for(cnt=ratings.size()-1;cnt>=1;cnt--)

        {

            if((ratings[cnt-1]>ratings[cnt])&&(candy[cnt]>=candy[cnt-1]))

                candy[cnt-1]=candy[cnt]+1;

        }

        cnt=accumulate(candy.begin(), candy.end(), 0);

        return cnt;

    }

};
```

### 区间问题
#### [435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)   (AAA)

这道题不要陷入查找重叠线段并删除的双层循环中，当第一遍排序结束后，前一个的终点大于后一个的起点就是重叠，同时只需一个变量记录上一个未重叠区间就可以了
```C++
class Solution {

public:

    int eraseOverlapIntervals(vector<vector<int>>& intervals) {

        sort(intervals.begin(),intervals.end(),

        [](vector<int> &a,vector<int>&b)

        {

            return a[1]<b[1];

        });

        // vector<int> prev;

        int cnt =0;

        int i   =0;

        vector<int> prev=intervals[i];

        for(i=1;i<intervals.size();i++)

        {

            if(prev[1]>intervals[i][0])

            {

                //overlaped

                cnt++;

            }

            else

            {

                prev=intervals[i];

            }

        }

        return cnt;

    }

};
```

### 练习题
#### [605. 种花问题](https://leetcode.cn/problems/can-place-flowers/) (A)
```c++
class Solution {

public:

    bool canPlaceFlowers(vector<int>& flowerbed, int n) {

        //For Loop to check

        //need shadow vector to record

        if(flowerbed.empty())

        {

            return true;

        }

        int cnt=0;

        if(flowerbed.size()==1)

        {

            if(flowerbed.front()==0)

            {

                cnt++;

            }

            else

            {

                ;

            }

        }

        else

        {

            vector<int> shadow = flowerbed;

            int i=1;

            if(shadow[0]==0&&shadow[1]==0)

            {

                shadow[0]=1;

                cnt++;

            }

            for(;i<shadow.size()-1;i++)

            {

                if(cnt>=n)

                {

                    break;

                }

                if(shadow[i]==1)

                {

                    continue;

                }

                else

                {

                    if(shadow[i-1]==0 && shadow[i+1]==0)

                    {

                        shadow[i]=1;    

                        cnt++;

                    }

                }

            }            

            if(shadow[i]==0&&shadow[i-1]==0)

            {

                shadow[i]=1;    

                cnt++;

            }

        }

        return cnt>=n;

    }

};
```
#### [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)（AA）
以各个区间末端点的从小到大排序。
``` c++
class Solution {

public:

    int findMinArrowShots(vector<vector<int>>& points) {

        if(points.empty())

        {

            return 0;

        }

        sort(points.begin(),points.end(),[](vector<int>&a ,vector<int>&b)

        {

            return a[1]<b[1];

        });

        vector<vector<int>> shadow = points;

        int cnt=0;

        int i=0;

        int j=0;

        int arrow=0;

        while(i<points.size())

        {

            arrow = points[i][1];

            cnt++;//一直箭

            while(i<points.size())

            {

                if(arrow<=points[i][1]&&arrow>=points[i][0])

                {

                    i++;

                }

                else

                {

                    // cnt++;//无法达到，需要新的箭

                    break;

                }

            }

  

        }

        return cnt;

    }

};
```

```c++
class Solution {

public:

    vector<int> partitionLabels(string s) {

        vector<vector<int>> vec;

        unordered_map<char, vector<int>> umap;

        int i=0;

        for(char a: s)

        {

            umap[a].push_back(i);

            i++;

        }

        for(unordered_map<char, vector<int>>::iterator it=umap.begin();it!=umap.end();it++)

        {

            sort(it->second.begin(),it->second.end());

            vec.push_back({it->second.front(),it->second.back()});

        }

        sort(vec.begin(),vec.end(),[](vector<int>&a,vector<int>&b)

        {

            // return a[1]<b[1];

            return a[0]<b[0];

        });

        vector<int> prev=vec[0];

        vector<int> ret;

        int left=INT_MAX;

        int right=0;

        i=0;

        left=left>=vec[i][0]?vec[i][0]:left;

        right=right<=vec[i][1]?vec[i][1]:right;

        for( i=1;i<vec.size();i++)

        {

            if(right>=vec[i][0])

            {

                //overlap

                left=left>=vec[i][0]?vec[i][0]:left;

                right=right<=vec[i][1]?vec[i][1]:right;

            }

            else

            {

                ret.push_back({right-left+1});

                right=vec[i][1];

                left=vec[i][0];

            }

        }

        ret.push_back({right-left+1});

        return ret;

    }

};
```
官方题解
``` c++
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int last[26];
        int length = s.size();
        for (int i = 0; i < length; i++) {
            last[s[i] - 'a'] = i;
        }
        vector<int> partition;
        int start = 0, end = 0;
        for (int i = 0; i < length; i++) {
            end = max(end, last[s[i] - 'a']);
            if (i == end) {
                partition.push_back(end - start + 1);
                start = end + 1;
            }
        }
        return partition;
    }
};

```

#### [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/) (A)
``` c++
class Solution {

public:

    int maxProfit(vector<int>& prices) {

        int i=0;

        int gain=0;

        for(i=0;i<prices.size()-1;i++)

        {

            if(prices[i]<prices[i+1])

            {

                gain+=prices[i+1]-prices[i];

            }

            else

            {

                continue;

            }

        }

        return gain;

    }

};
```

#### [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/) (AA)
以下是我自己的解法，这道题落入了细节陷阱。
我的思路，按照1号下标的的大小排序，1号下标越小，在最终的排序中越有可能在前面的位置。如果1号下标相同则身高矮的放前面。
之后使用一个buffer数组来保存
``` c++
class Solution {

public:

    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {

        if(people.empty())

        {

            return {};

        }

        vector<vector<int>> shadow;;//(people.size(),vector<int>(2,0));

        sort(people.begin(),people.end(),[](vector<int>&a,vector<int>&b)

        {

            if(a[1]!=b[1])

                return a[1]<b[1];

            else

                return a[0]<=b[0];

        });

        int i=0;

        int cnt=0;

        int j=0;

        for(i=0;i<people.size();i++)

        {

            if(people[i][1]==0)

            {

                //shadow[i]=people[i];

                shadow.push_back(people[i]);

            }

            else

            {

                int rec=0;

                cout<<people[i][0]<<","<<people[i][1]<<endl;

                cnt=0;

                for( j=0;j<=i-1;j++)

                {

                    if(shadow[j][0]>=people[i][0])

                    {

                        cnt++;

                        if(cnt==people[i][1])

                        {

                            rec=j;

                        }

                    }

                    if(shadow[j][0]<people[i][0])

                    {

                        rec++;

                    }

                }

                if(cnt<=people[i][1])

                {

                    shadow.push_back(people[i]);

                }

                else

                {

                    shadow.insert(shadow.begin()+rec+1,people[i]);

                }

                //shadow.insert(shadow.begin()+j+1,people[i]);

            }

        }

        //return people;

        return shadow;

    }

};
```

#### [665. 非递减数列](https://leetcode.cn/problems/non-decreasing-array/) （A）
改成非递减数组有两种策略，一种是v[i]=v[i+1], 一种是v[i+1]=v[i].
对于第一种策略，因为改变了i元素，所以要回过头去检查i-1；
```c++
class Solution {

public:

    bool checkPossibility(vector<int>& nums) {

        int i=0;

        vector<int> shad=nums;

        // vector<int> buf;

        int cnt1=0,cnt2=0;

        for( i=0;i<shad.size()-1;i++)

        {

            if(shad[i]<=shad[i+1])

            {

                continue;

            }

            else

            {

                cnt1++;

                shad[i+1]=shad[i];

                cout<<"1 change :"<<i+1<<endl;

                // i--;  //no need for forward direction

                // buf.push_back(i);

                if(cnt1>=2)

                {

                    break;

                }

            }

        }

        shad=nums;

        for( i=0;i<shad.size()-1;i++)

        {

            if(shad[i]<=shad[i+1])

            {

                continue;

            }

            else

            {

                cnt2++;

                shad[i]=shad[i+1];

                cout<<"2 change :"<<i<<endl;

                if(i!=0)

                    i=i-2;
                    //As we changed shad[i]. we should go back to check shad[i-1] and shad[i]

                // buf.push_back(i);

                if(cnt2>=2)

                {

                    break;

                }

            }

        }

        cout<<"cnt1 "<<cnt1<<","<<"cnt2 "<<cnt2<<endl;

        if(cnt1<=1 || cnt2 <=1)

        {

            return true;

        }

        else

        {

            return false;

        }

    }

};
```
## 二、双指针
### 指针基础
![[Pasted image 20230601184527.png]]
### 基础练习
#### [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/) (A)
最开始的想法是用umap去存所有值，然后查找。但是题目要求额外空间是常量
所以使用双指针（头尾指针）
```c++
class Solution {

public:

    vector<int> twoSum(vector<int>& numbers, int target) {

        int i=0;

        int j=numbers.size()-1;

        int flag=0;

        while(i<j)

        {

            int a=numbers[i];

            int b=numbers[j];

            int c=a+b;

            if(c>target)

            {

                j--;

            }

            else if(c<target)

            {

                i++;

            }

            else

            {

                flag=1;

                break;

            }

        }

        if(flag==0)

        {

            return {-1,-1};

        }

        else

        {

            return{++i,++j};

        }

    }

};
```

#### [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/) (A)
``` c++
class Solution {

public:

    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {

        int i=0;

        for(i=m;i<n+m;i++)

        {

            nums1[i]=nums2[i-m];

        }

        sort(nums1.begin(),nums1.end(),[](auto a, auto b)

        {

            return a<b;

        }

        );

        return;

    }

};
```

### 快慢指针
#### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

```c++
/**

 * Definition for singly-linked list.

 * struct ListNode {

 *     int val;

 *     ListNode *next;

 *     ListNode(int x) : val(x), next(NULL) {}

 * };

 */

class Solution {

public:

    ListNode *detectCycle(ListNode *head) {

        ListNode * p1,*p2;//p1 fast p, p2 slow p

        p1=p2=head;

        // p2=p1->next;

        //while()

        if(head==nullptr)

        {

            return NULL;

        }

        do

        {

            if(p1->next!=nullptr&&p1->next->next!=nullptr)

            {

                p1=p1->next->next;

            }

            else

            {

                //no loop

                return NULL;

            }

            if(p2->next!=nullptr&&p2->next->next!=nullptr)

            {

                p2=p2->next;

            }

            else

            {

                //no loop

                return NULL;

            }

            // if(p1==p2)

            // {

            //     break;

            // }

        }while(p1!=p2);

        p1=head;

        while(p1!=p2)

        {

            p1=p1->next;

            p2=p2->next;

        }

        return p1;

    }

};
``` 
### 滑动窗口
#### [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/) 
个人解法，超时了
```c++
class Solution {

public:

    string minWindow(string s, string t) {

        int left=0,right=s.size()-1;

        if(s.size()<t.size())

        {

            return "";

        }

        unordered_map<char,int>umapt;

        for(auto c :t)

        {

            umapt[c]++;

        }

        vector<string> vret;

        unordered_map<char,int>umapback(umapt) ;

        unordered_map<char,int>umaps;

        while(left<s.size())

        {

            umapt=umapback;

            if(umapt.find(s[left])==umapt.end())

            {

                left++;

                continue;

            }

            else

            {

                right=left;

                int flag=0;

                while(right<s.size())

                {

                    if(umapt.find(s[right])==umapt.end())

                    {

                        right++;

                        continue;

                    }

                    else

                    {

                        umapt[s[right]]--;

                        if(umapt[s[right]]==0)

                            umapt.erase(s[right]);

                        if(umapt.size()==0)

                        {

                            //cout<<s.substr(left,right-left+1)<<endl;

                            vret.push_back(s.substr(left,right-left+1));

                            break;

                        }

                    }                    

                    right++;

                }

            }

            left++;

        }

        if(vret.empty())

        {

            return "";

        }

        sort(vret.begin(),vret.end(),[](string &a, string &b)

        {

            return a.size()<b.size();

        });

        return vret.front();

    }

};
```
看完了题解后写的
```c++
class Solution {

public:

    string minWindow(string s, string t) {

        unordered_map<char,int>umapt;

        unordered_map<char,int>umaps;

        vector<int>flag(128,0);

        vector<int>rec={0};

        rec.push_back(0);

        for(auto c:t)

        {

            umapt[c]++;

            flag[c]=1;

        }

        int left=0;int right=0;

        int cnt=0;

        for(right=0;right<s.size();right++)

        {

            char cright=s[right];

            char cleft = s[left];

            if(flag[cright]==0)

            {

                continue;

            }

            else

            {

                umaps[cright]++;

                if(umaps[cright]==umapt[cright])

                {

                    cnt++;

                    // if(cnt==umapt.size())

                    // {

                    //     if(rec[1]>right-left+1)

                    //     {

                    //         rec[0]=left;

                    //         rec[1]=right-left+1;

                    //     }

                    // }

                }

            }

            while(cnt==umapt.size()&&left<=right)

            {

                cleft = s[left];

                if(flag[cleft]==0)

                {

                    left++;

                    continue;

                }

                umaps[cleft]--;

                if(umaps[cleft]<umapt[cleft])

                {

                    if(rec[1]>right-left+1 ||rec[1]==0)

                    {

                        rec[0]=left;

                        rec[1]=right-left+1;

                    }

                    cnt--;

                }

                left++;

            }

        }

        return s.substr(rec[0],rec[1]);

    }

};
```
### 练习
#### [633. 平方数之和](https://leetcode.cn/problems/sum-of-square-numbers/)
这道题终究是用了sqrt函数，解法不好，但是如果用费马平方和定理，解法太特殊
```c++
class Solution {

public:

    bool judgeSquareSum(int c) {

        long a=0;

        long b=sqrt(c);

        long m=0;

  

        while(a<=b)

        {

            m=(a*a+b*b);

            if(m>c)

            {

                b--;                

            }

            else if (m<c)

            {

                a++;

            }

            else

            {

                break;

            }

        }

        if(m==c)

        {

            cout<<a<<","<<b<<endl;

            return true;

        }

        else

        {

            return false;

        }

    }

};
```
#### [680. 验证回文串 II](https://leetcode.cn/problems/valid-palindrome-ii/)
这道题之所以用递归，是由于当出现不一致时，有两种选择，一种是去除左边的，一种是去除右边的。两种选择分支都需要分析。
```c++
class Solution {

public:

    bool validPalindrome(string s) {

        int flag=0;

        int ret = isEqual(s, 0, s.size()-1,flag);

        return ret;

    }

    bool isEqual(string &s,int left,int right,int flag)

    {

        if(flag>1)

        {

            return false;

        }

        if(left>=right)

        {

            return true;

        }

        bool bret1,bret2,bret;

        bret1=bret2=false;

        if(s[left]==s[right])

        {

            bret=isEqual(s, left+1, right-1,flag);

        }

        else

        {

            if(s[left+1]==s[right])

            {

                bret1 = isEqual(s, left+1, right,flag+1);

            }

            if(s[left]==s[right-1])

            {

                bret2 = isEqual(s, left, right-1,flag+1);

            }

        }

        if(bret==true || (bret1||bret2)==true)

        {

            return true;

        }

        else

        {

            return false;

        }

    }

  

};
```

#### [340. 至多包含 K 个不同字符的最长子串](https://leetcode.cn/problems/longest-substring-with-at-most-k-distinct-characters/) (AAA)
这道题是滑动窗口题，个人总结有如下技巧
- 滑动窗口移动的时候应该先扩大，再缩小。也就是right先加left后加
- left的收敛界限需要仔细判别
- k=0这种情况比较特殊，只能单独判断
```c++
class Solution {

public:

    int lengthOfLongestSubstringKDistinct(string s, int k) {

        int left,right;

        unordered_map<char,int> umap;

        vector<vector<int>> rvec;

        int maxlen=0;

        char c,t;

        left=right=0;

        int cnt=0;

        if(k==0)

        {

            return 0;

        }

        for(;right<s.size();right++)

        {

            c=s[right];

            umap[c]++;

            if(umap[c]==1)

            {

                cnt++;

            }

            if(cnt==k+1)

            {

                //from left -> right-1

                rvec.push_back({left,right-left});

                maxlen=max(right-left,maxlen);

                for(;left<right;left++)
                {

                    t=s[left];

                    if(umap[t]==1)

                    {

                        umap[t]--;

                        cnt--;

                        if(cnt==k)

                        {

                            left++;

                            break;

                        }

                    }

                    else

                    {

                        umap[t]--;

                    }

                }

            }

        }

        if(cnt<=k)

        {

            rvec.push_back({left,right-left});

            maxlen=max(right-left,maxlen);

        }

        return maxlen;

    }

};
```
这个解法更简洁，值得学习
``` java
int lengthOfLongestSubstringKDistinct(char * s, int k){
    int hash[200]={0};
    int left=0;
    int right=0;
    int ans=0;
    int cnt=0;
    int n=strlen(s);
    while(right<n){
        hash[s[right]]++;
        if(hash[s[right]]==1){
            cnt++;
        }
        while(cnt==k+1){
            hash[s[left]]--;
            if(hash[s[left]]==0){
                cnt--;
            }
            left++;
        }
        ans=fmax(ans,right-left+1);
        right++;
    }
    return ans;

}

作者：Harry666
链接：https://leetcode.cn/problems/longest-substring-with-at-most-k-distinct-characters/solution/hua-by-harry666-ggty/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
## 三、二分查找
### 基础练习
#### [69. x 的平方根](https://leetcode.cn/problems/sqrtx/)（AA）
二分查找时，区间缩短时的条件很重要。left=temp+1和right=temp-1 
```c++
class Solution {

public:

    int mySqrt(int x) {

        int left,right;

        left=right=0;

        long temp=0;

        right=x;

        while(left<=right)

        {

            temp=left+(right-left)/2;

            if(temp*temp<=x && (temp+1)*(temp+1)>x)

            {

                break;

            }

            else if(temp*temp>x)

            {

                right=temp-1;

            }

            else

            {

                left=temp+1;

            }

        }

        return temp;

    } 

};
```

## 区间查找
#### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)
```c++
class Solution {

public:

    vector<int> searchRange(vector<int>& nums, int target) {

        int left,right;

        left=0,right=nums.size()-1;

        int i=0;

        int flag=0;

        vector<int>vret;

        while(left<=right)

        {

            i=left+(right-left)/2;

            if(nums[i]==target)

            {

                flag=1;

                break;

            }

            else if(nums[i]<target)

            {

                left=i+1;

            }

            else

            {

                right=i-1;

            }

        }

        if(flag==0)

        {

            return {-1,-1};

        }

        int j=0;

        for(j=i;j>=0;j--)

        {

            if(nums[j]!=target)

            {

                break;

            }

        }

        vret.push_back(j+1);

        for(j=i;j<nums.size();j++)

        {

            if(nums[j]!=target)

            {

                break;

            }

        }

        vret.push_back(j-1);

        return vret;

    }

};
```
在二分时使用左闭右开或左开右闭可以得到上下界


#### [81. 搜索旋转排序数组 II](https://leetcode.cn/problems/search-in-rotated-sorted-array-ii/)
```c++
class Solution {

public:

    bool search(vector<int>& nums, int target) {

        sort(nums.begin(),nums.end());

        int left=0,right=nums.size()-1;

        int i=0;

        int flag=0;

        while(left<=right)

        {

            i=left+(right-left)/2;

            if(nums[i]==target)

            {

                flag=1;

                break;

            }

            else if(nums[i]<target)

            {

                left=i+1;

            }

            else

            {

                right=i-1;

            }

        }

        if(flag==0)

        {

            return false;

        }

        else

        {

            cout<<i<<endl;

            return true;

        }

    }

};
```

#### [540. 有序数组中的单一元素](https://leetcode.cn/problems/single-element-in-a-sorted-array/)
解法一：这种办法很慢
```c++
class Solution {

public:

    int singleNonDuplicate(vector<int>& nums) {

        unordered_map<int,int> umap;

        int ret=-1;

        for(int i : nums)

        {

            umap[i]++;

        }

        for(auto a : umap)

        {

            if(a.second==1)

            {

                ret=a.first;

            }

        }

        return ret;

    }

};
```
解法二
![[Pasted image 20230614152649.png]]
```c++
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        int low = 0, high = nums.size() - 1;
        while (low < high) {
            int mid = (high - low) / 2 + low;
            mid -= mid & 1;
            if (nums[mid] == nums[mid + 1]) {
                low = mid + 2;
            } else {
                high = mid;
            }
        }
        return nums[low];
    }
};

作者：LeetCode-Solution
链接：https://leetcode.cn/problems/single-element-in-a-sorted-array/solution/you-xu-shu-zu-zhong-de-dan-yi-yuan-su-by-y8gh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
#### [4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)
```c++
class Solution {

public:

    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {

        vector<int> nums=nums1;

        nums.insert(nums.end(),nums2.begin(),nums2.end());

        sort(nums.begin(),nums.end());

        int left=0,right=nums.size()-1;

        double mid=0;

        if((right-left)%2==0)

        {

            //odd

            mid=nums[left+(right-left)/2];

        }

        else

        {

            mid=(nums[left+(right-left)/2]+nums[left+(right-left)/2+1])/2.0;

        }

        return mid;

    }

};
```
## 四、排序算法
- [ ] 快速排序
- [ ] 归并排序
- [ ] 冒泡排序
- [ ] 桶排序
#### 快速排序
```c++
public static void quickSort(int[] arr, int startIndex, int endIndex) {
    // 递归结束条件：startIndex大于或等于endIndex时
    if (startIndex >= endIndex) {
        return;
    }
    // 得到基准元素位置
    int pivotIndex = partition(arr, startIndex, endIndex);
    // 根据基准元素，分成两部分进行递归排序
    quickSort(arr, startIndex, pivotIndex - 1);
    quickSort(arr, pivotIndex + 1, endIndex);
}
    /**
     * 分治（双边循环法）
     * @param arr 待交换的数组
     * @param startIndex 起始下标
     * @param endIndex 结束下标
     */
private static int partition(int[] arr, int startIndex, int endIndex) {
    // 取第1个位置（也可以选择随机位置）的元素作为基准元素
    int pivot = arr[startIndex];
    int left = startIndex;
    int right = endIndex;

    while( left != right) {
        //控制right指针比较并左移
        while(left<right && arr[right] > pivot){
            right--;
        }
        //控制left指针比较并右移
        while( left<right && arr[left] <= pivot) {
            left++;
        }
        //交换left和right指针所指向的元素
        if(left<right) {
            int p = arr[left];
            arr[left] = arr[right];
            arr[right] = p;
        }
    }
    //pivot和指针重合点交换
    arr[startIndex] = arr[left];
    arr[left] = pivot;
    return left;
}
public static void main(String[] args) {
    int[] arr = new int[] {4,4,6,5,3,2,8,1};
    quickSort(arr, 0, arr.length-1);
    System.out.println(Arrays.toString(arr));
}

作者：小灰
链接：https://leetcode.cn/leetbook/read/journey-of-algorithm/5r5yte/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
#### [75. 颜色分类](https://leetcode.cn/problems/sort-colors/)

快排的分段函数里要注意以下点：
1. 先right，后left
2. 判断while结束条件是< 而不是<=
3. 因为left的初值为start
4. left收缩时的第二个判断条件时left<=first
```c++
class Solution {

public:

    void sortColors(vector<int>& nums) {

        quicksort(nums,0,nums.size()-1);

  

    }

    void quicksort(vector<int>& nums,int start,int end)

    {

        if(start>=end)
        {

            return;
        }

  

        int pivot = partition(nums, start, end);

        quicksort(nums, start, pivot-1);

        quicksort(nums, pivot+1, end);

        return;

    }

    int partition(vector<int>& nums,int start,int end)

    {

        int left = start;

        int right= end;

        int first =nums[start];

        int temp=0;

        while(left!=right)

        {

            while(left<right && nums[right]>first)

            {

                right--;

            }

            while(left<right && nums[left]<=first)

            {

                left++;

            }

            if(left<right)

            {

                temp = nums[left];

                nums[left]= nums[right];

                nums[right] = temp;

            }

        }

        temp = nums[left];

        nums[left]= nums[start];

        nums[start] = temp;

        return left;

    }
};
```


## 五、搜索
### 深度优先搜索
#### [695. 岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/)
```c++
class Solution {

public:

    int maxAreaOfIsland(vector<vector<int>>& grid) {

        if(grid.empty())

        {

            return 0;

        }

        vector<vector<int>> sgrid = grid;

        int xsize=grid.front().size();

        int ysize=grid.size();

        int ret=0;

        int x=0,y=0;

        vector<int> varea;

  

        for(y=0;y<ysize;y++)

        {

            for(x=0;x<xsize;x++)

            {

                if(sgrid[y][x]==0)

                {

                    continue;

                }

                else

                {

                    int s=0;

                    s+=travel(sgrid,  x, y);

                    ret=max(s,ret);

                    //varea.push_back(s);//for debug

                }

            }

        }

        //sort(varea.begin(),varea.end());

        //return varea.back();

        return ret;

    }

    int travel(vector<vector<int>>& sgrid,int x,int y)

    {

        if(x<0 || x>=sgrid.front().size() || y<0 || y>=sgrid.size())

        {

            return 0;

        }

        if(sgrid[y][x]==0)

        {

            return 0;

        }

        int ret=1;

        sgrid[y][x]=0;

        ret+=travel(sgrid, x-1, y);

        ret+=travel(sgrid, x+1, y);

        ret+=travel(sgrid, x, y-1);

        ret+=travel(sgrid, x, y+1);

  

        return ret;

    }

};
```

#### [547. 省份数量](https://leetcode.cn/problems/number-of-provinces/)
```c++
class Solution {

public:

    int findCircleNum(vector<vector<int>>& isConnected) {

        vector<vector<int>> shadowTab = isConnected;

        int x,y;

        int num=0;

        for(y=0;y<shadowTab.size();y++)

        {

            for(x=0;x<=y;x++)

            {

                if(shadowTab[y][x]==0)

                {

                    continue;

                }

                num+=dfs(shadowTab,x,y);

            }

        }

        return num;

    }

    int dfs(vector<vector<int>>& shadowTab,int x,int y)

    {

        if(x<0||y<0|x>=shadowTab.front().size()||y>=shadowTab.size())

        {

            return 0;

        }

        if(shadowTab[y][x]==0)

        {

            return 0;

        }

        shadowTab[y][x]=0;

        for(int i=0;i<shadowTab.size();i++)

            dfs(shadowTab,y,i);

        return 1;

    }

};
```

#### [417. 太平洋大西洋水流问题](https://leetcode.cn/problems/pacific-atlantic-water-flow/)
```c++
#define DEBUG 0

class Solution {

public:

    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {

        vector<vector<int>> vret;

        int x,y;

        oheights=sheights1=sheights2=heights;

        for(y=0;y<heights.size();y++)

        {

            for(x=0;x<heights.front().size();x++)

            {

                int ret1,ret2;

                // sheights =heights;

                #if DEBUG

                cout<<"MAIN:("<<y<<","<<x<<")"<<endl;

                #endif

                ret1=isLinktoPac(x,y);

                // sheights=heights;

                ret2=isLinktoAtl(x,y);

                if(ret1==1 && ret2 ==1)

                {

                    #if DEBUG

                    cout<<"MAIN PUSH:("<<y<<","<<x<<")"<<endl;

                    #endif

                    vret.push_back({y,x});

                }

            }

        }

        return vret;

    }

    int isLinktoPac(int x,int y)

    {

        vector<vector<int>>& heights =sheights1;

        #if DEBUG

        cout<<"PAC:("<<y<<","<<x<<")"<<endl;

        #endif

        if(x<0||y<0||x>=heights.front().size()||y>=heights.size())

        {

            return 0;

        }

        if(heights[y][x]==-1)

        {

            return 1;

        }

        if(x==0||y==0)

        {

            //reach PAC

            heights[y][x]=-1;

            return 1;

        }

        if(heights[y][x]==INT_MAX)

        {

            return 0;

        }

        int temp = oheights[y][x];

        heights[y][x]=INT_MAX;

        if(y<heights.size()-1 && oheights[y+1][x]<=temp)

        {

            int ret;

            ret = isLinktoPac(x,y+1);

            if(ret)

            {

                heights[y][x]=-1;

                return 1;

            }

        }

        if(x<heights.front().size()-1 && oheights[y][x+1]<=temp)

        {

            int ret;

            ret = isLinktoPac(x+1,y);

            if(ret)

            {

                heights[y][x]=-1;

                return 1;

            }

        }

        if(y>0 && oheights[y-1][x]<=temp)

        {

            int ret;

            ret = isLinktoPac(x,y-1);

            if(ret)

            {

                heights[y][x]=-1;

                return 1;

            }

        }

        if(x>0 && oheights[y][x-1]<=temp)

        {

            int ret;

            ret = isLinktoPac(x-1,y);

            if(ret)

            {

                heights[y][x]=-1;

                return 1;

            }

        }

        heights[y][x]=temp;

        return 0;

    }

    int isLinktoAtl(int x,int y)

    {

        vector<vector<int>>& heights =sheights2;

        #if DEBUG

        cout<<"Alt:("<<y<<","<<x<<")"<<endl;

        #endif

        if(x<0||y<0||x>=heights.front().size()||y>=heights.size())

        {

            return 0;

        }

        if(heights[y][x]==-1)

        {

            return 1;

        }

        if(x==heights.front().size()-1||y==heights.size()-1)

        {

            //reach Atl

            heights[y][x]=-1;

            return 1;

        }

        if(heights[y][x]==INT_MAX)

        {

            return 0;

        }

        int temp = oheights[y][x];

        heights[y][x]=INT_MAX;

        if(y<heights.size()-1 && oheights[y+1][x]<=temp)

        {

            int ret;

            ret = isLinktoAtl(x,y+1);

            if(ret)

            {

                heights[y][x]=-1;

                return 1;

            }

        }

        if(x<heights.front().size()-1 && oheights[y][x+1]<=temp)

        {

            int ret;

            ret = isLinktoAtl(x+1,y);

            if(ret)

            {

                heights[y][x]=-1;

                return 1;

            }

        }

        if(y>0 && oheights[y-1][x]<=temp)

        {

            int ret;

            ret = isLinktoAtl(x,y-1);

            if(ret)

            {

                heights[y][x]=-1;

                return 1;

            }

        }

        if(x>0 && oheights[y][x-1]<=temp)

        {

            int ret;

            ret = isLinktoAtl(x-1,y);

            if(ret)

            {

                heights[y][x]=-1;

                return 1;

            }

        }

        heights[y][x]=temp;

        return 0;

    }

    private:

    vector<vector<int>> sheights1,sheights2,oheights;

};
```
### 回溯法
#### [46. 全排列](https://leetcode.cn/problems/permutations/)
回溯的关键是利用递归的性质保存每一层的特征，如果用解法一，最重要的就是backtrace中使用start和end来确定这次回溯里新加如的变量。解法二直接在原数组修改，更高效。

解法一：
```c++
class Solution {

public:

    vector<vector<int>> permute(vector<int>& nums) {

        vector<vector<int>> vret;

        backtrace(vret,nums);

        return vret;

    }

    int backtrace(vector<vector<int>> &vret,vector<int>& vnums)

    {

        // vector<vector<int>> vret;

        if(vnums.size()==1)

        {

            vret.push_back(vnums);

            return 0;

        }

        else

        {

            for(int i=0;i<vnums.size();i++)

            {

                int start=vret.size();//!!!!

                int temp=vnums[i];

                vnums.erase(vnums.begin()+i, vnums.begin()+i+1);

                backtrace(vret,vnums);

                vnums.emplace(vnums.begin()+i,temp);

                int end= vret.size();//!!!!

                for(int j=start;j<end;j++)

                {

                    vret[j].push_back(temp);

                }

            }            

        }

        return 0;

    }

};
```
解法二：
```C++
class Solution {
public:
    void backtrack(vector<vector<int>>& res, vector<int>& output, int first, int len){
        // 所有数都填完了
        if (first == len) {
            res.emplace_back(output);
            return;
        }
        for (int i = first; i < len; ++i) {
            // 动态维护数组
            swap(output[i], output[first]);
            // 继续递归填下一个数
            backtrack(res, output, first + 1, len);
            // 撤销操作
            swap(output[i], output[first]);
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int> > res;
        backtrack(res, nums, 0, (int)nums.size());
        return res;
    }
};
```
#### [77. 组合](https://leetcode.cn/problems/combinations/)
```c++
//这种解法使用了上一题的思路，但是太麻烦，会超时
class Solution {

private:

    vector<int> nums;

    int limit=0;

public:

    vector<vector<int>> combine(int n, int k) {

        for(int i=1;i<=n;i++)

        {

            nums.push_back(i);

        }

        vector<vector<int>> vret;

        vector<vector<int>> vtemp;

        limit =k;

        backtrace(0, vret);

        for(vector<int>& c : vret)

        {

            sort(c.begin(),c.end());

        }

        sort(vret.begin(),vret.end(),[](vector<int>& a,vector<int>& b)

        {

            // if(a.front()==b.front())

            // {

            //     return a.back()<b.back();

            // }

            // else

            // {

            //     return a.front()<b.front();

            // }

            for(int i=0;i<a.size();i++)

            {

                if(a[i]==b[i])

                {

                    continue;

                }

                else

                {

                    return a[i]<b[i];

                }

            }

            return a.front()<b.front();

        });

  

        for(int i=0;i<vret.size();i++)

        {

            if(i==0)

            {

                vtemp.push_back(vret[i]);

            }

            else

            {

                // if(vret[i][0]==vtemp.back()[0]&&vret[i][1]==vtemp.back()[1])

                // {

                //     continue;

                // }

                vector<int> & a=vret[i];

                vector<int> & b=vtemp.back();

                int iflag=0;

                for(int j=0;j<a.size();j++)

                {

                    if(a[j]==b[j])

                    {

                        continue;

                    }

                    else

                    {

                        vtemp.push_back(vret[i]);

                        break;

                    }

                }               

            }

        }

        return vtemp;

    }

    int backtrace(int cnt, vector<vector<int>> &vret)

    {

        if(cnt>limit)

        {

            return 0;

        }

        int scnt=cnt+1;

        int start=0;

        int end=0;

        if(cnt==limit-1)

        {

            for(int i=0;i<nums.size();i++)

            {

                vret.push_back({nums[i]});

            }

            return 0;

        }

        else

        {            

            for(int i=0;i<nums.size();i++)

            {

                start=vret.size();

                int temp = nums[i];

                nums.erase(nums.begin()+i,nums.begin()+i+1);

                backtrace(scnt, vret);

                nums.emplace(nums.begin()+i,temp);

                end=vret.size();

                for(int j=start;j<end;j++)

                {

                    vret[j].push_back(temp);

                }

            }

        }

        return 0;

    }

};

```

```c++
//改进后可以了，需要额外的标志位来记录下一次搜索的位置
class Solution {

private:

    vector<int> nums;

    int limit=0;

public:

    vector<vector<int>> combine(int n, int k) {

        for(int i=1;i<=n;i++)

        {

            nums.push_back(i);

        }

        vector<vector<int>> vret;

        vector<vector<int>> vtemp;

        limit =k;

        backtrace(0, vret,0);

        return vret;

    }

    int backtrace(int cnt, vector<vector<int>> &vret,int loc)

    {

        if(cnt>limit)

        {

            return 0;

        }

        int scnt=cnt+1;

        int start=0;

        int end=0;

        if(cnt==limit-1)

        {

            for(int i=loc;i<nums.size();i++)

            {

                vret.push_back({nums[i]});

            }

            return 0;

        }

        else

        {            

            for(int i=loc;i<nums.size();i++)

            {

                start=vret.size();

                int temp = nums[i];

                // nums.erase(nums.begin()+i,nums.begin()+i+1);

                backtrace(scnt, vret,i+1);

                // nums.emplace(nums.begin()+i,temp);

                end=vret.size();

                for(int j=start;j<end;j++)

                {

                    vret[j].push_back(temp);

                }

            }

        }

        return 0;

    }

};
```

```c++
//参考解法
class Solution {

public:

    // 主函数

    vector<vector<int>> combine(int n, int k)

    {

        vector<vector<int>> ans;

        vector<int> comb(k, 0);

        int count = 0;

        backtracking(ans, comb, count, 1, n, k);

        return ans;

    }

    // 辅函数

    void backtracking(vector<vector<int>>& ans, vector<int>& comb, int& count, int pos, int n, int k)

    {

        if (count == k) {

        ans.push_back(comb);

        return;

        }

        for (int i = pos; i <= n; ++i)

        {

            comb[count++] = i; // 修改当前节点状态

            backtracking(ans, comb, count, i + 1, n, k); // 递归子节点

            --count; // 回改当前节点状态

        }

    }

};
```

#### [79. 单词搜索](https://leetcode.cn/problems/word-search/)
```c++
class Solution {

public:

    vector<vector<char>> backboard;

    bool find=0;

    bool exist(vector<vector<char>>& board, string word) {

        int x=0;int y=0;

        bool bret;

        backboard= board;

        if(word.empty())

        {

            return false;

        }

        for(int y=0;y<board.size();y++)

        {

            for(int x=0;x<board.front().size();x++)

            {

  

                bret=backtrace(board, word, x, y, 0);

                // board = backboard;

                if(bret)

                    return true;

            }

        }

        return bret;

    }

    bool backtrace(vector<vector<char>>& board, string word,int x,int y,int cnt)

    {

        if(find)

        {

            return true;

        }

        if(cnt==word.size())

        {

            find = true;

            return true;            

        }

        if(x<0 || x>=board.front().size() || y<0 ||y>=board.size())

        {

            return false;

        }

        if(board[y][x]==word[cnt])

        {

            bool ret[4];

            char temp = board[y][x];

            board[y][x]=' ';

            ret[0] = backtrace(board, word, x, y+1, cnt+1);

            ret[1] = backtrace(board, word, x, y-1, cnt+1);

            ret[2] = backtrace(board, word, x+1, y, cnt+1);

            ret[3] = backtrace(board, word, x-1, y, cnt+1);

            board[y][x]=temp;

            bool bret=0;

            for(bool btemp : ret)

            {

                bret=bret|btemp;

            }

            if(bret)

            {

                return true;

            }

        }

        return false;

    }

};
```
### 广度优先搜索
``` c++

```