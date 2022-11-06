# 人工智慧課堂作業:notebook:
在此紀錄CS188 Fall 2022的吃豆人解題過程
1. DFS演算法用於吃豆人純走迷宮
2. BFS演算法用於吃豆人純走迷宮
## 重要程式碼功能:wrench:
回傳一個初始起點 : 
`problem.getStartState()`

找出該點的鄰近點，它會回傳三個數值
1. 鄰近點(子節點)
2. 移動方向(EAST、SOUTH、WEST、NORTH)
3. 移動步數或是COST???(因為沒用到就沒特別注意了:v:)

``problem.getSuccessors(node)``

判斷該點是否到達終點 : 
```problem.isGoalState(node):```

util工具包 : 
內有許多作者先寫完的資料結構(也可自行用append()、pop()...取代)
## DFS(Depth-First Search)
### 概念
在此使用stack實作，使用stack的特性**FILO**，當這個點要找出子節點，子節點會繼續找並將找到點放入stack最上面，直到宣告這個點找不到終點時就拔掉往前一點看，看前一點有沒有到終點可能。

要用**這個點能不能導航到終點**的看法去看，從stack中抓出來的這點能不能跑到終點，所以要用**當下**這個node去看，如果這個node跑不到終點就拔掉，再從stack抓一個。

### 實作卡住點
剛開始想說node的stack、尋訪過的點(visited)、移動步驟(actions)各創一個list，但完全忘記**一個點對應到一個動作**的概念，而將其他點的動作也一起append到stack，造成其他不相關的動作也加到actions中...真的吐了，難怪移動一直失敗🤮

一開始寫法 :
```
stack = util.Stack()
stack.push(problem.getStartState())
visited = list()
actions = list()
while not stack.isEmpty():
    node = stack.pop()
...
for neighbor, move, _ in problem.getSuccessors(node):
    visited.append(node)
    if neighbor not in visited:
        actions.append(move)
        stack.push(neighbor)
```
上面的stack觀念是對的沒錯，但那個actions直接將一堆不相干的動作加入

後來改善的寫法 :
```
stack = util.Stack()
    stack.push((problem.getStartState(), []))
    visited = list()
    while not stack.isEmpty():
        node, actions = stack.pop()
        if problem.isGoalState(node):
            return actions
            
        for neighbor, move, _ in problem.getSuccessors(node):
            visited.append(node)
            if neighbor not in visited:
                stack.push((neighbor, actions+[move]))
    return []
```
actions改成**基於前一點可能可以到終點**，因此一開始node和actions要被pop出來，然後在後面push將前一點pop出來的資訊繼續疊上去，如果該點找不到終點，會被pop出來後再用前一點看有沒有其他路線可走。

## BFS(Breadth-First Search)
### 概念
用queue的FIFO的特性，強迫將前一點所有子節點找到才能往下一層尋訪。

基本上跟上面的DFS找node的問題差不多 只是從stack變成queue。
### BFS的圖
![](https://github.com/alianlbj23/AiProjectInClass/blob/main/bfs%E6%98%AF%E6%84%8F%E5%9C%96.png?raw=true)

## 程式碼參考網址
[DFS、BFS觀念程式碼參考](https://github.com/jeknov/aiAlgorithmsWithPacman/blob/master/01.Search_BFS.DFS.UCS.Astar/search.py)
