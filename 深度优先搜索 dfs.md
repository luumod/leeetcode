# 深度优先搜索 dfs



深度优先搜索的本质：递归



传送门：

[](https://leetcode.cn/problems/flood-fill/description/)

dfs求解：递归到达每一个格子，判断这个格子的颜色是不是我们需要更改的颜色curcolor，如果是，则把他更改为目标颜色color，并且借助它再次递归，遍历它周围的四个方向的格子，重复进行判断与更新。

直到所有的格子都遍历之后，需要更改的格子都已经递归到了并且更改了，不需要更改颜色的颜色则根本不会递归到这个格子。

```cpp
class Solution {
private:
    const int dirX[4]{0,0,-1,1};
    const int dirY[4]{-1,1,0,0};
public:
    void dfs(vector<vector<int>>& image,int sr,int sc,int curcolor,int color)
    {
        //判断是否是需要更改的颜色
        if (image[sr][sc]==curcolor)
        {
            //首先更改此颜色
            image[sr][sc]=color;
            //接着递归遍历其周围的四个方向，递归下去
            for (int i=0;i<4;i++)
            {
                int mx=sr+dirX[i];
                int my=sc+dirY[i];
                if (mx>=0 && mx<image.size() && my>=0 && my<image[0].size())
                {
                    dfs(image,mx,my,curcolor,color);
                }
            }
        }   
    }
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        int curcolor=image[sr][sc];
        if (curcolor==color)
        {
            return image;
        }
        dfs(image,sr,sc,curcolor,color);
        return image;
    }
};
```







# 广度优先搜索 bfs



广度优先搜索的本质：利用**队列**先进先出的特性



传送门：

[](https://leetcode.cn/problems/flood-fill/description/)

bfs求解：当我们遍历到某一个格子的时候，把这个格子存储进队列中，当队列不为空的时候，我们重复取得队列 的第一个元素，然后pop一次。再对这个元素进行四个方向的判断，并且把需要更改的格子入队，等待下次取得该元素。别忘了队列的特性：先进先出，所以我们下一次取得的元素是之前的，接着再重复进行循环，直到队列为空。



```cpp
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
        const int dirX[4]{0,0,-1,1};
        const int dirY[4]{-1,1,0,0};
        int curcolor=image[sr][sc];
        if (curcolor==color)
        {
            return image;
        }
        queue<pair<int,int>> q;
        q.push(make_pair(sr,sc));
        image[sr][sc]=color;
        while (!q.empty())
        {
            pair<int,int> num=q.front();  //获取队列顶部元素
            q.pop();    //弹出队列顶部元素
            for (int i=0;i<4;i++)
            {
                int mx=num.first+dirX[i];
                int my=num.second+dirY[i];
                if (mx>=0 && mx<image.size() && my>=0 && my<image[0].size() &&
                    image[mx][my]==curcolor)
                {
                    q.push(make_pair(mx,my));
                    image[mx][my]=color;
                }
            }
        }
        return image;
    }
};
```





