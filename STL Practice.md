### 数组
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


#### [240. 搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/)
```c++
class Solution {

public:

    bool searchMatrix(vector<vector<int>>& matrix, int target)

    {

        int y=0,x=0;

        bool bret=0;

        for(y=0;y<matrix.size();y++)

        {

            if(target<matrix[y].front() || target>matrix[y].back())

            {

                continue;

            }

            bret = b_search(matrix[y],target);

            if(bret)

            {

                break;

            }

        }

        return bret;

    }

    bool b_search(vector<int> & vec,int target)

    {

        int left=0,right=vec.size()-1;

        int piv=left+(right-left)/2;

        while(left<=right)

        {

            if(vec[piv]>target)

            {

                right = piv-1;

            }

            else if(vec[piv]<target)

            {

                left = piv+1;

            }

            else

            {

                return true;

            }

            piv=left+(right-left)/2;

        }

        return false;

    }

};
```



### 栈
#### [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)
```c++
class MyQueue {

private:

    stack<int> stack1;

    stack<int> stack2;

    int qtop=0;

    int flag=0;

public:

    MyQueue() {

        ;

    }

    void push(int x) {

        if(flag==0)

        {

            qtop=x;

            flag=1;

        }

        stack1.push(x);

    }

    int pop() {

        while(!stack1.empty())

        {

            int temp = stack1.top();

            stack2.push(temp);

            stack1.pop();

        }

        int ret = stack2.top();

        stack2.pop();

        flag=0;

        while(!stack2.empty())

        {

            int temp = stack2.top();

            push(temp);

            stack2.pop();

        }

        return ret;

    }

    int peek() {

        return qtop;

    }

    bool empty() {

        return stack1.empty();

    }

};

  

/**

 * Your MyQueue object will be instantiated and called as such:

 * MyQueue* obj = new MyQueue();

 * obj->push(x);

 * int param_2 = obj->pop();

 * int param_3 = obj->peek();

 * bool param_4 = obj->empty();

 */
```

#### [155. 最小栈](https://leetcode.cn/problems/min-stack/)
```c++
class MinStack {

public:

    stack<int> stackMain;

    stack<int> stackMini;    

    MinStack() {

  

    }

    void push(int val) {

        if(stackMini.empty())

        {

            stackMain.push(val);

            stackMini.push(val);

        }

        else

        {

            if(val<=stackMini.top())

            {

                stackMini.push(val);

            }

            stackMain.push(val);

        }

    }

    void pop() {

        int temp=stackMain.top();

        if(temp>stackMini.top())

        {

        }

        else if(temp<stackMini.top())

        {

            cout<<"error"<<endl;

        }

        else

        {

            stackMini.pop();

        }

        stackMain.pop();

    }    

    int top() {

        return stackMain.top();

    }

    int getMin() {

        return stackMini.top();

    }

};

  

/**

 * Your MinStack object will be instantiated and called as such:

 * MinStack* obj = new MinStack();

 * obj->push(val);

 * obj->pop();

 * int param_3 = obj->top();

 * int param_4 = obj->getMin();

 */
```
#### spare

### 优先队列

#### [23. 合并 K 个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)
``` c++
/**

 * Definition for singly-linked list.

 * struct ListNode {

 *     int val;

 *     ListNode *next;

 *     ListNode() : val(0), next(nullptr) {}

 *     ListNode(int x) : val(x), next(nullptr) {}

 *     ListNode(int x, ListNode *next) : val(x), next(next) {}

 * };

 */

class Solution {

public:

    ListNode* mergeKLists(vector<ListNode*>& lists) {

        priority_queue<int,vector<int>,less<int>> pque;

        int cnt=0;

        ListNode* ptemp;

        for(cnt=0;cnt<lists.size();cnt++)

        {

            ptemp= lists[cnt];

            while(ptemp!=nullptr)

            {

                pque.push(ptemp->val);

                ptemp=ptemp->next;

            }

        }

        ListNode * piv = nullptr;

        while(!pque.empty())

        {

            int temp = pque.top();

            ptemp = new ListNode(temp);

            ptemp->next=piv;

            piv=ptemp;

            pque.pop();

        }

        return piv;

    }

};
```
