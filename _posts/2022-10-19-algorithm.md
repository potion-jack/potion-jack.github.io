---
layout: post
title: Today I learned(2022_10_19)
date: 2022-10-19
tags: [TIL, algorithm, bfs, dfs]
toc:  true
---
dfs and bfs by myself<br/>
{: .message }

## dfs

```
def dfs(row,col):
    n = len(grid)
    m = len(grid[0])
    if grid[row][col] == '0':
        return False

    elif grid[row][col] == '1':
        need_visit = [[row,col]]
        grid[row][col] = '0'
        while need_visit:
            row,col = need_visit.pop()
            for drow,dcol in [[1,0],[-1,0],[0,1],[0,-1]]:
                if (0 <= row+drow) & (row+drow <= n-1) & (0 <= col+dcol) & (col+dcol <= m-1):
                    dfs(row+drow,col+dcol)
        return True

count = 0
for i in range(len(grid)):
    for j in range(len(grid[0])):
        if dfs(i,j):
            count += 1
print(count)

```

## bfs

```
def bfs(row,col):
    n = len(grid)
    m = len(grid[0])
    if grid[row][col] == '0':
        return False
    else:
        need_visit = [[row,col]]
        while need_visit:
            row,col = need_visit.pop()

            grid[row][col]='0'
            for drow,dcol in [[1,0],[-1,0],[0,1],[0,-1]]:
                if (0 <= row+drow) and (row+drow <= n-1) and (0 <= col+dcol) and (col+dcol <= m-1):
                    if grid[row+drow][col+dcol] == '1':
                        need_visit.append([row+drow,col+dcol])
                        grid[row+drow][col+dcol] = '0'
        return True
```
