# Rotten Yes Hannu ✅

The code Striver wrote (BFS with queue + visited array) is the standard optimal solution for the Rotten Oranges problem. Let me break it down for you clearly:

⸻

🔹 Complexity
	•	Time Complexity:
	•	Every cell (at most n \times m) is inserted into the queue at most once.
	•	For each cell, we check 4 directions.
	•	⏱️ Overall: O(n × m)
	•	Space Complexity:
	•	vis[n][m] + queue (can hold at most n × m elements).
	•	🗂️ Overall: O(n × m)

This is the best you can do, because you must look at all cells anyway.

⸻

🔹 Can it be Improved?

Yes, but only slightly in code style, not in complexity:
	1.	Remove vis[][]:
Instead of maintaining a separate visited matrix, you can directly modify the grid itself (change 1 → 2 when it gets rotten). This saves space and makes the code cleaner.
	•	Space drops to O(1) extra (ignoring queue).
	•	Still O(n × m) time.
	2.	Multi-source BFS is already optimal:
Starting BFS from all rotten oranges at once is the best way (because infection spreads simultaneously).

⸻

🔹 Most Optimized Code (without vis[][])

#include <bits/stdc++.h>
using namespace std;

int orangesRotting(vector<vector<int>>& grid) {
    int n = grid.size(), m = grid[0].size();
    queue<pair<pair<int,int>, int>> q; 
    int cntFresh = 0;

    // Step 1: Push all initially rotten oranges
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < m; j++) {
            if(grid[i][j] == 2) {
                q.push({{i,j}, 0});
            }
            if(grid[i][j] == 1) cntFresh++;
        }
    }

    int tm = 0, cnt = 0;
    int drow[] = {-1, 0, 1, 0};
    int dcol[] = {0, 1, 0, -1};

    // Step 2: BFS
    while(!q.empty()) {
        int r = q.front().first.first;
        int c = q.front().first.second;
        int t = q.front().second;
        q.pop();

        tm = max(tm, t);

        for(int i = 0; i < 4; i++) {
            int nr = r + drow[i], nc = c + dcol[i];
            if(nr >= 0 && nr < n && nc >= 0 && nc < m && grid[nr][nc] == 1) {
                grid[nr][nc] = 2; // rot it
                q.push({{nr, nc}, t+1});
                cnt++;
            }
        }
    }

    return (cnt == cntFresh) ? tm : -1;
}

int main() {
    vector<vector<int>> grid = {
        {2,1,1},
        {1,1,0},
        {0,1,1}
    };

    cout << "Minimum time required: " << orangesRotting(grid) << endl;
}


⸻

✅ This is the most optimized form of Striver’s code — same logic, but no extra vis[][].

⸻

Do you want me to dry run both versions side by side so you can see why vis[][] is not needed?
## 🔍 Problem Recap

You're given a grid:
- `0` → empty cell  
- `1` → fresh orange  
- `2` → rotten orange  

Each minute, rotten oranges infect adjacent fresh ones (up/down/left/right). Return the **minimum time** to rot all oranges, or `-1` if impossible.

---

## ✅ Approach Summary

This code uses **multi-source BFS**:
- All rotten oranges are pushed into a queue initially.
- Each level of BFS represents one minute.
- Fresh oranges are counted (`tot`), and each time one rots, `cnt` is incremented.
- If `cnt == tot` at the end, all oranges rotted → return `days`.
- Else, return `-1`.

---

## 🧠 Why This Works

- BFS spreads layer by layer, simulating simultaneous infection.
- Each level of the queue represents oranges rotting at the same time.
- DFS would go deep and not simulate time correctly without extra logic.

---

## 🧪 Dry Run with Grid

Initial Grid:
```
2 1 1
1 1 0
0 1 1
```

### Step 1: Initialization

- `tot = 7` (all non-zero cells)
- Rotten queue: `[(0,0)]`
- `cnt = 0`, `days = 0`

---

### Step 2: Minute 1

Process rotten `(0,0)`  
→ Infect `(0,1)` and `(1,0)`

New rotten queue: `[(0,1), (1,0)]`  
`cnt = 1` → now 3 rotten oranges  
`days = 1`

Grid:
```
2 2 1
2 1 0
0 1 1
```

---

### Step 3: Minute 2

Process `(0,1)` → infect `(0,2)`  
Process `(1,0)` → infect `(1,1)`

New rotten queue: `[(0,2), (1,1)]`  
`cnt = 3` → now 5 rotten oranges  
`days = 2`

Grid:
```
2 2 2
2 2 0
0 1 1
```

---

### Step 4: Minute 3

Process `(0,2)` → no fresh neighbors  
Process `(1,1)` → infect `(2,1)`

New rotten queue: `[(2,1)]`  
`cnt = 5` → now 6 rotten oranges  
`days = 3`

Grid:
```
2 2 2
2 2 0
0 2 1
```

---

### Step 5: Minute 4

Process `(2,1)` → infect `(2,2)`

New rotten queue: `[(2,2)]`  
`cnt = 6` → now 7 rotten oranges  
`days = 4`

Grid:
```
2 2 2
2 2 0
0 2 2
```

---

### Step 6: End

Queue is empty.  
`cnt = tot = 7` → ✅ all oranges rotted  
Return `days = 4`

---

## 🧾 Code Explanation

```cpp
int orangesRotting(vector<vector<int>>& grid) {
    if(grid.empty()) return 0;

    int m = grid.size(), n = grid[0].size();
    int days = 0, tot = 0, cnt = 0;
    queue<pair<int, int>> rotten;

    // Count total oranges and push rotten ones
    for(int i = 0; i < m; ++i){
        for(int j = 0; j < n; ++j){
            if(grid[i][j] != 0) tot++;
            if(grid[i][j] == 2) rotten.push({i, j});
        }
    }

    // Directions: right, left, down, up
    int dx[4] = {0, 0, 1, -1};
    int dy[4] = {1, -1, 0, 0};

    // BFS level by level
    while(!rotten.empty()){
        int k = rotten.size();
        cnt += k;

        while(k--){
            int x = rotten.front().first;
            int y = rotten.front().second;
            rotten.pop();

            for(int i = 0; i < 4; ++i){
                int nx = x + dx[i], ny = y + dy[i];
                if(nx < 0 || ny < 0 || nx >= m || ny >= n || grid[nx][ny] != 1) continue;
                grid[nx][ny] = 2;
                rotten.push({nx, ny});
            }
        }

        if(!rotten.empty()) days++;
    }

    return tot == cnt ? days : -1;
}
```

---

## 🧪 Output

```cpp
int main() {
    vector<vector<int>> v{ {2,1,1} , {1,1,0} , {0,1,1} };
    int rotting = orangesRotting(v);
    cout << "Minimum Number of Minutes Required " << rotting << endl;
}
```

**Output:**
```
Minimum Number of Minutes Required 4
```

---

If you'd like, I can help you visualize this with actual grid diagrams and arrows like Striver does in his slides — just say the word!
<img width="1470" height="956" alt="Screenshot 2025-09-05 at 8 18 07 PM" src="https://github.com/user-attachments/assets/897abb65-5f2e-4ee7-b97b-b26bbca8b41c" />
https://takeuforward.org/data-structure/rotten-oranges-min-time-to-rot-all-oranges-bfs/ 



Absolutely! Here's a full breakdown of your C++ code in **Striver-style**, with diagrams, dry run, and explanation of each step — including why this approach works and how it differs slightly from the classic BFS version.

---

## 🔍 Problem Recap

You're given a grid:
- `0` → empty cell  
- `1` → fresh orange  
- `2` → rotten orange  

Each minute, rotten oranges infect adjacent fresh ones (up/down/left/right). Return the **minimum time** to rot all oranges, or `-1` if impossible.

---

## ✅ Approach Summary

This code uses **multi-source BFS**:
- All rotten oranges are pushed into a queue initially.
- Each level of BFS represents one minute.
- Fresh oranges are counted (`tot`), and each time one rots, `cnt` is incremented.
- If `cnt == tot` at the end, all oranges rotted → return `days`.
- Else, return `-1`.

---

## 🧠 Why This Works

- BFS spreads layer by layer, simulating simultaneous infection.
- Each level of the queue represents oranges rotting at the same time.
- DFS would go deep and not simulate time correctly without extra logic.

---



<img width="1470" height="956" alt="Screenshot 2025-09-05 at 8 18 07 PM" src="https://github.com/user-attachments/assets/897abb65-5f2e-4ee7-b97b-b26bbca8b41c" />

use visted marircix and queue
<img width="1470" height="956" alt="Screenshot 2025-09-05 at 8 16 02 PM" src="https://github.com/user-attachments/assets/9a4d742c-5983-4f86-8511-f9df40072cb8" />


<img width="1470" height="956" alt="Screenshot 2025-09-05 at 8 16 11 PM" src="https://github.com/user-attachments/assets/d2c28763-adf8-48b5-b1c7-0a6c35f1cc96" />

<img width="1470" height="956" alt="Screenshot 2025-09-05 at 8 17 08 PM" src="https://github.com/user-attachments/assets/6e8ffe15-b47f-489a-ac8b-f9ce6cc9eefc" />
 <img width="1470" height="956" alt="Screenshot 2025-09-05 at 8 19 03 PM" src="https://github.com/user-attachments/assets/a4fd4826-1d49-4d4a-a561-b27fb8c88734" />

<img width="1470" height="956" alt="Screenshot 2025-09-05 at 8 22 06 PM" src="https://github.com/user-attachments/assets/183ae04e-2a2e-4530-b83c-c9a90bd3b4f1" />



<img width="1470" height="956" alt="Screenshot 2025-09-05 at 8 19 49 PM" src="https://github.com/user-attachments/assets/395f9ff4-1629-41ce-bcd4-a3b5fefac8c9" />



<img width="1470" height="956" alt="Screenshot 2025-09-05 at 8 21 44 PM" src="https://github.com/user-attachments/assets/eb6cbfec-4909-4ab9-a0e5-3ecc042983f7" />



# Step-by-Step Striver-Style Solution

## 1. Problem Statement

Given an \(N \times M\) grid where  
- 0 represents an empty cell  
- 1 represents a fresh orange  
- 2 represents a rotten orange  

Every minute, any fresh orange adjacent (up/down/left/right) to a rotten one becomes rotten. Return the minimum number of minutes that must elapse until no cell has a fresh orange. If it’s impossible, return \(-1\).

---

## 2. Intuition

We treat all rotten oranges as **multiple BFS sources**.  

By enqueueing every rotten orange at time 0, we spread the “infection” level by level. BFS naturally processes nodes in order of their distance (time) from the start, ensuring each fresh orange is rotten at the earliest possible minute.

---

## 3. Detailed Steps

1. Initialize:  
   
   - A queue of tuples \((r, c, t)\) for row, column, and the minute at which it rots.  
   - A counter `freshCount` to track total fresh oranges.  
   - A 2D `visited` array (or reuse the grid) to mark processed cells.  
   - Four direction arrays `delRow = {-1, 0, 1, 0}` and `delCol = {0, 1, 0, -1}`.  

2. Enqueue all rotten oranges with time 0 and mark them visited. Increment `freshCount` for each fresh orange encountered.

3. Perform BFS:  
   
   While the queue isn’t empty:  
   - Pop \((r, c, t)\).  
   - Update `maxTime = max(maxTime, t)`.  
   - For each of the 4 directions:  
     - Compute neighbor \((nr, nc)\).  
     - If within bounds, unvisited, and is a fresh orange:  
       - Mark visited (or set grid[nr][nc] = 2).  
       - Enqueue \((nr, nc, t + 1)\).  
       - Decrement `freshCount`.  

4. After BFS, if `freshCount > 0` return \(-1\). Otherwise, return `maxTime`.

---

## 4. Dry Run Example

Grid:  
```
2 1 1
1 1 0
0 1 1
```

- Initial queue: \((0,0,0)\), `freshCount = 5`.  
- Minute 1: Infect \((0,1)\) and \((1,0)\). Queue holds \((0,1,1)\), \((1,0,1)\).  
- Minute 2: Infect \((0,2)\), \((1,1)\), \((2,1)\). Queue holds those with t=2.  
- Minute 3: Infect \((2,2)\). Queue holds \((2,2,3)\).  

All oranges rotten by minute 3 → answer = 3.

---

## 5. C++ Implementation

```cpp
#include <bits/stdc++.h>
using namespace std;

int orangesRotting(vector<vector<int>>& grid) {
    int n = grid.size(), m = grid[0].size();
    queue<tuple<int,int,int>> q;
    int freshCount = 0, maxTime = 0;
    
    // Direction vectors
    int delRow[4] = {-1, 0, 1, 0};
    int delCol[4] = {0, 1, 0, -1};
    
    // 1. Initialize queue with rotten oranges
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 2) {
                q.emplace(i, j, 0);
            } else if (grid[i][j] == 1) {
                freshCount++;
            }
        }
    }
    
    // 2. BFS to rot adjacent fresh oranges
    while (!q.empty()) {
        auto [r, c, t] = q.front();
        q.pop();
        maxTime = max(maxTime, t);
        
        for (int k = 0; k < 4; k++) {
            int nr = r + delRow[k];
            int nc = c + delCol[k];
            
            if (nr >= 0 && nr < n && nc >= 0 && nc < m
                && grid[nr][nc] == 1) {
                grid[nr][nc] = 2;         // mark as rotten
                freshCount--;
                q.emplace(nr, nc, t + 1);
            }
        }
    }
    
    // 3. Check if any fresh oranges remain
    return (freshCount > 0) ? -1 : maxTime;
}

// Example usage
int main() {
    vector<vector<int>> grid = {
        {2,1,1},
        {1,1,0},
        {0,1,1}
    };
    cout << orangesRotting(grid) << "\n";  // Output: 3
    return 0;
}
```

---

## 6. Complexity Analysis

- Time Complexity: \(\mathcal{O}(N \times M)\) — each cell is processed at most once in BFS.  
- Space Complexity: \(\mathcal{O}(N \times M)\) — for the queue and grid modifications.

---

Feel free to explore variations of multi-source BFS in problems like “Walls and Gates” or “Distance to Nearest Zero” to reinforce this pattern!

## Problem Statement

Given a 2D grid of size \(N \times M\), each cell can be:
- 0: empty  
- 1: fresh orange  
- 2: rotten orange  

Every minute, any fresh orange adjacent (up/down/left/right) to a rotten one becomes rotten. Compute the minimum minutes required until no fresh oranges remain. If some fresh oranges can never rot, return \(-1\).【10^】

---

## Key Observations

- Multiple rotten oranges act as **simultaneous sources** of contamination.
- We need the **shortest time** for every fresh orange to get infected.
- A breadth-first search (BFS) from all rotten oranges at once ensures we process “layers” of infection in time order.  

---

## Approach: Multi-Source BFS

1. Enqueue all rotten oranges with timestamp 0.  
2. Maintain a `visited` matrix (or reuse the grid) to avoid revisiting.  
3. While queue is not empty:
   - Dequeue \((r,c,t)\).  
   - For each of its 4 neighbours \((nr,nc)\):
     - If inside bounds and is a fresh orange and unvisited:
       - Mark visited/rotten.
       - Enqueue \((nr, nc, t+1)\).
       - Track the maximum time seen so far.
4. After BFS, scan grid for any remaining fresh orange:
   - If found, return \(-1\).
   - Otherwise, return the maximum time recorded.【10^】

---

## Why BFS Works

- BFS explores cells in **order of increasing distance (time)** from the sources.
- All oranges at distance \(d\) from any initial rotten cell get processed together at minute \(d\).
- This guarantees the first time we reach a fresh orange is the **minimum** possible.  

---

## Dry Run

Consider this grid and track \((row, col, time)\) in the queue:

```
2 1 1
1 1 0
0 1 1
```

1. Initial queue: \((0,0,0)\).  
2. Minute 1: rotten \((0,1)\) and \((1,0)\).  
3. Minute 2: newly rotten \((0,2)\), \((1,1)\), \((2,1)\).  
4. Minute 3: rotten \((2,2)\).  

All oranges rot by minute 3 → answer = 3.

---

## Pseudocode

```text
function orangesRotting(grid):
    N ← number of rows
    M ← number of columns
    queue ← empty queue of (r, c, time)
    visited ← N×M boolean matrix initialized false
    freshCount ← 0

    // 1. Initialize queue with all rotten oranges
    for r in 0…N-1:
      for c in 0…M-1:
        if grid[r][c] == 2:
          queue.enqueue((r, c, 0))
          visited[r][c] ← true
        else if grid[r][c] == 1:
          freshCount ← freshCount + 1

    maxTime ← 0
    directions ← [(-1,0), (0,1), (1,0), (0,-1)]

    // 2. BFS over four-way neighbours
    while queue not empty:
      (r, c, t) ← queue.dequeue()
      maxTime ← max(maxTime, t)

      for (dr, dc) in directions:
        nr ← r + dr
        nc ← c + dc
        if 0 ≤ nr < N and 0 ≤ nc < M and not visited[nr][nc] and grid[nr][nc] == 1:
          visited[nr][nc] ← true
          queue.enqueue((nr, nc, t + 1))
          freshCount ← freshCount - 1

    // 3. Check if any fresh oranges remain
    if freshCount > 0:
      return -1
    return maxTime
```

---

## Complexity Analysis

- Time Complexity: \(\mathcal{O}(N \cdot M)\)  
  (Each cell enqueued at most once; scanning neighbours is constant.)  
- Space Complexity: \(\mathcal{O}(N \cdot M)\)  
  (Queue and visited matrix.)

---
# Step-by-Step Striver-Style Addendum

---

## 1. Why Use BFS Over DFS

Breadth-first search (BFS) is the natural choice for this problem because we’re looking for the **minimum number of minutes** that it takes for all fresh oranges to rot. BFS processes nodes in “layers” or “levels”:

- All rotten oranges at minute 0 infect their neighbors simultaneously, forming level 1.  
- Those newly rotten oranges infect their neighbors at minute 1, forming level 2.  

This level-by-level expansion directly corresponds to time steps. As soon as a fresh orange is reached by BFS, you know it’s the **earliest possible** time it can rot.

By contrast, a depth-first search (DFS) would follow one path as deep as possible before backtracking. DFS cannot naturally track the shortest time from *all* sources in parallel without additional complex bookkeeping. You’d have to simulate a global clock or record and compare many time values, making the solution both error-prone and inefficient.

---

## 2. Key Insights and Important Points

- Multi-source BFS  
  Initialize the queue with **all** rotten oranges at time 0. This models the simultaneous spread from multiple points.

- Time Tracking  
  Store the current minute in each queue entry. The maximum timestamp dequeued is the answer.

- Grid Modification vs. Visited Array  
  You can mark a fresh orange as rotten in the original grid (`grid[nr][nc] = 2`) to avoid revisiting, eliminating a separate visited array.

- Four-Direction Movement  
  Use `delRow = {-1,0,1,0}` and `delCol = {0,1,0,-1}` to explore up, right, down, and left.

- Fresh Orange Counter  
  Keep a counter for fresh oranges and decrement it each time you rot one. If any remain at the end, return -1 immediately—no extra scan needed.

- Edge Cases  
  1. No fresh oranges at all → return 0.  
  2. No rotten oranges but some fresh ones → return -1.  
  3. Single cell grid.  

---

## 3. Complete C++ Implementation

```cpp
#include <bits/stdc++.h>
using namespace std;

int orangesRotting(vector<vector<int>>& grid) {
    int n = grid.size(), m = grid[0].size();
    queue<tuple<int,int,int>> q;
    int freshCount = 0, maxTime = 0;

    // Direction vectors for up, right, down, left
    int delRow[4] = {-1, 0, 1, 0};
    int delCol[4] = {0, 1, 0, -1};

    // 1. Initialize queue with all rotten oranges at time 0
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 2) {
                q.emplace(i, j, 0);
            } else if (grid[i][j] == 1) {
                freshCount++;
            }
        }
    }

    // 2. Multi-source BFS
    while (!q.empty()) {
        auto [r, c, t] = q.front();
        q.pop();
        maxTime = max(maxTime, t);

        for (int k = 0; k < 4; k++) {
            int nr = r + delRow[k];
            int nc = c + delCol[k];

            // If neighbor is within bounds and is a fresh orange
            if (nr >= 0 && nr < n && nc >= 0 && nc < m
                && grid[nr][nc] == 1) {
                grid[nr][nc] = 2;   // mark as rotten
                freshCount--;
                q.emplace(nr, nc, t + 1);
            }
        }
    }

    // 3. Check if any fresh oranges remain
    return (freshCount > 0) ? -1 : maxTime;
}

int main() {
    vector<vector<int>> grid = {
        {2, 1, 1},
        {1, 1, 0},
        {0, 1, 1}
    };
    cout << orangesRotting(grid) << "\n";  // Expected output: 3
    return 0;
}
```

---

## 4. Final Takeaways

- BFS guarantees the shortest time to reach each node from multiple sources.  
- DFS is unsuitable for minimum-distance or minimum-time across a grid when multiple roots exist.  
- Always consider whether your problem involves level-by-level expansion (BFS) or deep exploration (DFS).  

Next, you can explore similar patterns in problems like “Walls and Gates” or “01 Matrix” to see multi-source BFS in action again.

