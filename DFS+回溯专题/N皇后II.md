# N皇后II

[52. N皇后II](https://leetcode.cn/problems/n-queens-ii/description/) 

## 题目大意
N皇后问题变种，求出能摆放的方法总数

## 方法一、DFS暴搜

### 思路
1. 对于N皇后问题的DFS方法，由于王后有三种攻击方式，所以需要开三个布尔数组储存
   危险节点
2. 观察到一行能且只能放一个皇后，本题以行为单位搜索

### 整体代码
```
class Solution {
public:
    int ans = 0;

    void dfs(vector<bool> &m, vector<bool> &l, vector<bool> &r, 
            int deep, int n){
        if(deep == n){
            ans += 1;
            return;
        }
        for(int i=0;i<n;i++){
            if(!m[i] && !l[i+deep+2] && !r[i-deep+n]){
                m[i] = true, l[i+deep+2] = true, r[i-deep+n] = true;
                dfs(m, l, r, deep+1, n);
                m[i] = false, l[i+deep+2] = false, r[i-deep+n] = false;
            }
        }
        return;
    }

    int totalNQueens(int n) {
        vector<bool> m(25, false);
        vector<bool> l(25, false);
        vector<bool> r(25, false);
        dfs(m, l, r, 0, n);
        return ans;
    }
};
```

### 细节分析
1. `if(!m[i] && !l[i+deep+2] && !r[i-deep+n])`由于`vector`数组只有正指针，只要保证
   所有直线/斜线的王后在一个位置即可，不一定非要从0开始
2. `vector<bool> m(25, false);`定义`vector`时要先大小、后默认值

### 时空复杂度分析
* 树的深度为 $n$, 每个节点的子节点为 $n\sim1$,叶子节点个数为 $n!$，时间复杂度为
  $O(n!)$
* 空间复杂度为 $O(n)$
