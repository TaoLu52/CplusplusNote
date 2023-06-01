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
![[Pasted image 20230601184527.png]]
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
