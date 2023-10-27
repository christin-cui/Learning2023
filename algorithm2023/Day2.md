# 👉学习内容：977.有序数组的平方 ，209.长度最小的子数组 ，59.螺旋矩阵II 
做题建议：先独立做题，然后看视频讲解，然后看文章讲解，然后在重新做一遍题，把题目AC，最后整理成今日当天的博客

# 一、力扣[第977题](https://leetcode.cn/problems/squares-of-a-sorted-array/)有序数组的平方（[文章链接](https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html) [视频链接](https://www.bilibili.com/video/BV1QB4y1D7ep)）

> 我的思路：只想到把负数找出来变成相反数，排序后再平方，排序用双指针。但是这样和平方后排序其实没有什么改进，复杂度还是而很大。

> 本题思路：由于本题有序排列，平方后首尾两端为整个数组最大的，形成“大小大”的结构。所以，为了减少复杂度，可以两个指针直接比较首尾两端的值得到数组最大的值，再继续循环比较首尾两端，直到得到一个排序的新数组。
![在这里插入图片描述](https://img-blog.csdnimg.cn/34ce94dd34bd48e4a5ecf73d7c292710.png)

**自己手敲错误记录：**
- 【数组忘记定义】定义一个新数组result，与nums同长度，设所有元素为0   ：vector<int> result(nums.size(), 0);
- 【for的语句用法为定义变量；判断；操作】分号不能乱用，不能写成“int i=0 ; int j=nums.size()-1;” 且也不能写成“int i=0,int j =nums.size()-1;”
- 又忘记写返回值return（返回数组只写数组名即可）。
- 一开始定义的数组名称叫"new"，与变量冲突报错。

**本题语法特殊点：**
- 此处的i++和j--由于只能操作一个，且依据实际情况左右两端谁大，所以不能写在括号里可以不写（c++中），但是判断句“i<=j”后面的分号还要写。
- "result[k]=xxxx;k--;"可以简写为“result[k--]=xxxx;”

==小总结：==
- 代码开不了头时，一般为定义个变量，开始for循环，先写着。
- 好的解法复杂度不会高。想要找到好的思路，就要考虑到每题的特殊性，或者怎么利用一些已有的条件去简化算法操作流程，比如本题可以提前判断出来两头最大，就不需要再让算法一个一个对比找到最大的数。

**本题解答**
```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int k= nums.size() -1;
        vector<int> result(nums.size(), 0);
        for(int i=0, j=nums.size()-1;i<=j;){
            if(nums[i]*nums[i]<nums[j]*nums[j]){
                result[k--]=nums[j]*nums[j];
                j--;
            }
            else{
                result[k--]=nums[i]*nums[i];
                i++;
            }
        }
        return result;

    }
};
```

# 二、力扣[第209题](https://leetcode.cn/problems/minimum-size-subarray-sum/)长度最小的子数组（[文章链接](https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html)  [视频链接](https://www.bilibili.com/video/BV1tZ4y1q7XE)）
> 我的思路：没有思路，感觉乱七八糟，c++语法薄弱。

>本题思路：滑动窗口思想，前提本题是有序数组。图解参考代码随想录上述文章链接处。

**自己手敲错误记录：**
- 【while用法】
		问题：纠结while循环第一次后时出去执行for的"j++"还是whlie的判断"sum>s"？
		例如：
		[1 2 4 5]，s=4，则第一次应该是比较得到长度3，[1 2 4]>4；第二次是2，[2 4]>4。
		但是如果执行的是for的j++，则j+1向右移动一位，得到了3 [2 4 5]>4，错误。
>while 循环执行逻辑：
只要循环成立条件为真，就进入循环体
重复这个步骤，直到循环成立条件为假	
	
		💡事实上是while的语法不清晰，参考上述逻辑就清楚为什么题解在得到result长度值后，立刻写一句“sum -= nums[i++];”
		因为此时得到了一个新的sum，对于本例来说新的sum（此时只向右移动了i，为[2 4]）仍然在循环条件"while (sum >= s)"内，
		那么就可以继续执行while循环体,直到不满足了才出去执行j++。

- 新的变量总是忘记先int定义。


**本题语法特殊点：**
- 使用while语句而不是if语句，为了能够持续的执行for循环，而不至于if判断不成立后直接结束。
- for语句的索引j代表终止位置，减少算法计算量，否则和暴力解法无异（待补充）。


==小总结：==
- 关于复杂度：不要见到两个循环嵌套就以为时间复杂度是O(n^2)，时间复杂度主要是看每一个元素被操作的次数，每个元素在滑动窗口往右扩大时进来一次，窗口缩小时出去一次，每个元素都是被操作两次，所以时间复杂度是 2 × n 也就是O(n)。
- “滑动窗口本质上也是一种双指针法，是在充分理解题目的情况下，暴力算法的一种简化。这道题之所以可以使用滑动窗口，很重要的一个原因是，在移动终止位置的时候，初始位置是不可逆的，初始位置只可能往后移动，而不用每次都从第零个元素开始。所有双指针法，都是充分利用题目的一个隐藏的特征，来对暴力算法的一种简化。”

**本题解答**
```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int result= INT32_MAX;
        int sum =0;
        int i=0;
        int newlenth =0;
        for(int j=0;j<nums.size();j++){
            sum +=nums[j];
            while(sum >= target){
                newlenth=(j-i+1);
                result=min(result,newlenth);
                sum -= nums[i++];
            }
        }
        return result == INT32_MAX ? 0 : result;//不懂这句什么意思
    }
};
```


# 三、力扣[第59题](https://leetcode.cn/problems/spiral-matrix-ii/)螺旋矩阵II （[文章链接](https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html) [视频链接](https://www.bilibili.com/video/BV1SL4y1N7mV/)）

> 我的思路：看错了题目^ - ^，笑，还一本正经画了个图
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/2a47273d75b7497b8ec7a379b11e6f9a.png)


>本题思路：结合二分查找的循环不变量原则，在每次for循环的时候，需要采用共同的规则（左闭右开）来控制边界，以防元素溢出或混乱。

**自己手敲错误记录：**
- （待补充）

**本题特殊点：**
- 弄清楚每条边循环起始位置及结果的数组表达方式
- 由于是个圈，所以从哪开始绕都能闭合，初始值自定义（startx,starty）

==小总结：==
- 先找规律：圈数一定是n/2，因为绕一圈走两行
- 中心点（如果n为奇数）一定是(n/2,n/2)

**本题解答**
```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n, vector<int>(n, 0)); // 使用vector定义一个二维数组
        int startx = 0, starty = 0; // 定义每循环一个圈的起始位置
        int loop = n / 2; // 每个圈循环几次，例如n为奇数3，那么loop = 1 只是循环一圈，矩阵中间的值需要单独处理
        int mid = n / 2; // 矩阵中间的位置，例如：n为3， 中间的位置就是(1，1)，n为5，中间位置为(2, 2)
        int count = 1; // 用来给矩阵中每一个空格赋值
        int offset = 1; // 需要控制每一条边遍历的长度，每次循环右边界收缩一位
        int i,j;
        while (loop --) {
            i = startx;
            j = starty;

            // 下面开始的四个for就是模拟转了一圈
            // 模拟填充上行从左到右(左闭右开)
            for (j = starty; j < n - offset; j++) {
                res[startx][j] = count++;
            }
            // 模拟填充右列从上到下(左闭右开)
            for (i = startx; i < n - offset; i++) {
                res[i][j] = count++;
            }
            // 模拟填充下行从右到左(左闭右开)
            for (; j > starty; j--) {
                res[i][j] = count++;
            }
            // 模拟填充左列从下到上(左闭右开)
            for (; i > startx; i--) {
                res[i][j] = count++;
            }

            // 第二圈开始的时候，起始位置要各自加1， 例如：第一圈起始位置是(0, 0)，第二圈起始位置是(1, 1)
            startx++;
            starty++;

            // offset 控制每一圈里每一条边遍历的长度
            offset += 1;
        }

        // 如果n为奇数的话，需要单独给矩阵最中间的位置赋值
        if (n % 2) {
            res[mid][mid] = count;
        }
        return res;
    }
};
```

