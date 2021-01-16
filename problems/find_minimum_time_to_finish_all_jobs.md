## find_minimum_time_to_finish_all_jobs

### idea
This is a NP-hard problem, thus, need to brute force the reuslt\\
There are some ideas to optimise the result 
1. store the currect best result and stop when excedding
2. all worker are equivalent, when allocating a new job to a free worker, only need to allocate that job once.
3. further optimisations can be done by setting the limit of the work time and do binary search, it is more likely to stop earlier.

### apprach
Do DFS, and keep pruning branches while searching\\
**sorting does not guarantee to give a better solution, it depends on the data**

#### analysis
* Time complexity -> O(n^k) where n is the number of jobs, and k is the numebr of workers
  * for binary search approach, it runs faster in practice though it runs multiple times.
* Space complexity -> O(k) the space for storing the allocation, but need more space for stacking calling.

#### code
**dfs pruning**
``` python
self.best = sum(jobs)
        jobs.sort(reverse=False)
        alloc = [0] * k
        def dfs(i):
            # allocation the ith job
            if i == len(jobs):
                self.best = min(self.best, max(alloc))
                return
            for w in range(k):
                if alloc[w] + jobs[i] > self.best:
                    continue
                alloc[w] += jobs[i]
                dfs(i+1)
                alloc[w] -= jobs[i]
                if alloc[w] == 0:
                    break
        dfs(0)
        return self.best
```
**with binary search**
``` python
jobs.sort(reverse=True)
        def dfs(i):
            # allocation the ith job
            if i == len(jobs):
                return True
            for w in range(k):
                if alloc[w] + jobs[i] > limit:
                    continue
                alloc[w] += jobs[i]
                if dfs(i+1):
                    return True
                alloc[w] -= jobs[i]
                if alloc[w] == 0:
                    return False
            return False
        left = 0
        right = sum(jobs)
        while left < right:
            limit = (left + right) // 2
            alloc = [0] * k
            if dfs(0):
                right = limit
            else:
                left = limit + 1
        return left
```
<br>

[back](../index.md)