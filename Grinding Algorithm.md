## 贪心算法
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

#### [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/) ()
``` c++
```