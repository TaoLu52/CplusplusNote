#### [448. 找到所有数组中消失的数字](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/)
```c++
class Solution {

public:

    vector<int> findDisappearedNumbers(vector<int>& nums) {

        int n= nums.size();

        vector<int> vec(n,0);

        vector<int>ret;

        int i=0;

        for(i=0;i<nums.size();i++)

        {

            int index= nums[i]-1;

            vec[index]++;

        }

        for(i=0;i<nums.size();i++)

        {

            if(vec[i]==0)

            {

                ret.push_back(i+1);

            }

        }

        return ret;

    }

};
```
#### [48. 旋转图像](https://leetcode.cn/problems/rotate-image/)
```c++
//使用了额外矩阵
class Solution {

public:

    void rotate(vector<vector<int>>& matrix) {

        vector<int> temp;

        vector<vector<int>> shadow;

        int size = matrix.size();

        for(int i=0; i < size; i++)

        {

            temp.clear();

            for(int j=size-1; j >=0; j--)

            {

                temp.push_back(matrix[j][i]);

            }

            shadow.push_back(temp);

        }

        matrix=shadow;

        return;

    }

};
```

