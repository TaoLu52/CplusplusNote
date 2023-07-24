## Mind map
![[STL MindMap.canvas]]
## Knows
### Priority_queue
[C++ STL: priority_queue (Complete Guide) (workat.tech)](https://workat.tech/problem-solving/tutorial/cpp-stl-priority-queue-complete-guide-bnwvwagtxrws)
https://zhuanlan.zhihu.com/p/478887055

#### MaxHeap:
priority_queue < int > pq; //max-heap
```c++
#include<iostream>
#include<queue>
using namespace std;

int main() {
  priority_queue < int > pq; //max-heap
  for (int i = 0; i < 5; i++) {
    pq.push(i + 1);
  }
  cout << "The size of queue is : " << pq.size() << '\n';
  cout << "The highest priority element of queue is " << pq.top() << '\n';
  pq.pop();
  pq.pop();
  pq.push(10);
  pq.push(20);
  pq.push(30);
  if (!pq.empty()) {
    cout << "The new size of queue is " << pq.size() << '\n';
  }
  cout << "After inserting and deleting elements, the queue is " << '\n';
  while (!pq.empty()) {
    cout << pq.top() << " ";
    pq.pop();
  }
}
```
#### Mini Heap:
priority_queue < int, vector < int > , greater < int >> pq; 
```c++
#include<iostream>
#include<queue>
using namespace std;

int main() {
  priority_queue < int, vector < int > , greater < int >> pq; //min-heap
  for (int i = 0; i < 5; i++) {
    pq.push(i + 1);
  }
  cout << "The size of queue is : " << pq.size() << '\n';
  cout << "The highest priority element of queue is " << pq.top() << '\n';
  pq.pop();
  pq.pop();
  pq.push(10);
  pq.push(20);
  pq.push(30);
  if (!pq.empty()) {
    cout << "The new size of queue is " << pq.size() << '\n';
  }
  cout << "After inserting and deleting elements, the queue is " << '\n';
  while (!pq.empty()) {
    cout << pq.top() << " ";
    pq.pop();
  }
}
```


## Practices
[[STL Practice]]