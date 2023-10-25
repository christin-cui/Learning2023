## 👉学习内容：数组理论基础；力扣第704题 二分查找；力扣第27题 移除元素
 1. 数组理论基础  [文章链接](https://programmercarl.com/%E6%95%B0%E7%BB%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
 
 2. [704. 二分查找](https://leetcode.cn/problems/binary-search/)  [文章讲解](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html) [视频讲解](https://www.bilibili.com/video/BV1fA4y1o715)
题目建议： 熟悉 根据 左闭右开，左闭右闭 两种区间规则 写出来的二分法。
拓展题型：
 [35.搜索插入位置](https://leetcode.cn/problems/search-insert-position/)
 [34.在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

3. [27. 移除元素](https://leetcode.cn/problems/remove-element/)  
[文章讲解](https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html) 
[视频讲解](https://www.bilibili.com/video/BV12A4y1Z7LP)
题目建议：先把暴力写法写一遍。双指针法是本题的精髓。 

## 一、数组理论基础
1. **算法知识**
- 内存地址（连续）、数组内容、下标（从0开始）
- 16进制中,8+4=c=12（可以少占一位便于表示）
![啥](https://img-blog.csdnimg.cn/71cd5f378474457b91de6fecc44a9b32.png)
- 数组元素不能删除，只能靠覆盖来操作

2. **C++语法知识**

①*测试代码：*
```cpp
void test_arr() {
    int array[2][3] = {
		{0, 1, 2},
		{3, 4, 5}
    };
    cout << &array[0][0] << " " << &array[0][1] << " " << &array[0][2] << endl;
    cout << &array[1][0] << " " << &array[1][1] << " " << &array[1][2] << endl;
}

int main() {
    test_arr();
}
```
②*语法解释：*
> 这段C++代码定义了一个函数test_arr()和一个主函数main()，test_arr()函数中创建了一个二维整数数组array，并在主函数中调用了test_arr()函数。下面是每行代码的解释：
> 
> - void test_arr() { -  
> 这是一个函数的开始，函数名为test_arr，返回类型为void，表示该函数不返回任何值。
> 
> - int array[2][3] = { -
> 这一行声明了一个二维整数数组array，它有两行和三列。二维数组可以被看作是一个矩阵，其中有两行和每行有三个整数元素。
> 
> - cout << &array[0][0] << " " << &array[0][1] << " " << &array[0][2] << endl; -
> 这一行用于输出数组array的第一行的每个元素的地址（指针），并且在元素地址之间添加空格，最后输出一个换行符。&array[0][0]表示第一行第一列元素的地址，&array[0][1]表示第一行第二列元素的地址，以此类推。
> 
> - int main() { - 
> 这是主函数的开始，返回类型为int。
> 
> - test_arr(); - 
> 这一行调用了test_arr()函数，这将执行test_arr()函数中的代码。

## 二、704. 二分查找
>使用前提:数组为有序数组，同时题目还强调数组中无重复元素，因为一旦有重复元素，使用二分查找法返回的元素下标可能不是唯一的

 **💡关键问题在于对边界区间及开闭的处理：**
①是>=还是>    <mark>【只有左闭右闭才是等于，如：[1,1]合法】<mark>

②是middle还是middle-1     <mark>【首先明确if里middle数值不等于target，所以替换时middle不能留在更新边界后的区间内。即：左闭右闭时都去掉；左闭右开时，右开本身不包含不用再去掉。】<mark>

1. **复杂度分析**
-  时间复杂度：O(log⁡n)，其中 n是数组的长度。
-  空间复杂度：O(1)。

2. **两种区间思路**
* 注：midlle与边界一方对比时都是纯大于小于号

<mark>①**左闭右闭速记**<mark>
* nums【-1】
* while 【<=】
* 右-1 ；左+1  （更新边界时）
> int right = nums.size() - 1;   // 定义target在左闭右闭的区间里，[left, right]

>   while (left <= right) {    // 当left==right，区间[left, right]依然有效，所以用 <=
    
> if (nums[middle] > target) {
>   right = middle - 1; // target 在左区间，所以[left, middle - 1]   

>  } else if (nums[middle] < target) {
> left = middle + 1; // target 在右区间，所以[middle + 1, right]


<mark>②**左闭右开速记**<mark>
* nums【=】
* while 【<】
* 右= ；左边同上


> int right = nums.size(); // 定义target在左闭右开的区间里，即：[left, right)

        
>while (left < right) { // 因为left == right的时候，在[left, right)是无效的空间，所以使用 <
        
>if (nums[middle] > target) {
right = middle; // target 在左区间，在[left, middle)中

>} else if (nums[middle] < target) {
left = middle + 1; // target 在右区间，在[middle + 1, right)中

3. **704题解**

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
    //int nums[] ={-1,0,3,5,9,12};
        int left = 0;
        int right = nums.size() - 1;
        while (left <= right) {
            int middle =(left + right)/2;
            //int middle = left + ((right - left) / 2);合并同类项后相同，写法不同，防止溢出
            if (nums[middle] > target){
                right = middle - 1;
            }else if (nums[middle] < target){
                left = middle +1;
            }else {
                return middle;
            }
        }
        return -1;
    }
};
```

<mark>💥易混淆：注意二分法原理、本题要求、left、right 以及 middle均为下标，只有nums[middle] 和target对应的为实际数值元素。

## 三、27. 移除元素

1. **关于数组空间：**
-  数组是连续的，所以“移除”元素事实上是（后面的元素往前移动）“覆盖”元素，所以本题中本身在末尾的"val"值是不处理的，仅仅在返回size变量值（对元素计数操作）时进行减法，但实际上在size个元素后面可能还会有其他元素。如数组[1，3，5，2，3，2] 移除val = 2后，返回的数组个数size=4，减少两个，但是最终的数组为[1，3，5，3，**2**] 。不要误以为最后的2被删除了，因为智能覆盖。
3. **在做题时是否直接调用库函数，要综合考虑两点：**
- 面试题目要看，该题目的是否为了考察原理，如果库只是其中的一小步，那就可以调用
- 实际应用时，要综合考虑调用具体库的<mark>复杂度<mark>(所以要对复杂度有概念)。例如：erase函数看似是一步解决，但是其复杂度不是“删除”O(1)，而是“覆盖”O(n)。
4. **暴力解法思路**
- 一层for循环用来找元素
- 一层for循环用来覆盖前面的元素
5. **双指针<mark>速记<mark>**
- 快指针去找萝卜，慢指针挖坑。慢指针填完一个萝卜挖一个坑（slow ++），即慢指针填上一个元素就会指向下一个空位处，所以最后返回的slow值正好对应于前面的元素个数。slow为**下标**从0开始，最终的slow对应于最后一个没填的坑。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/85477b323bac4c33abb9ddf4ef8ac311.png)
 5. **题解**
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow = 0;
        for(int fast =0; fast < nums.size(); fast++){
            if (nums[fast] != val){
                nums[slow] =nums[fast];
                slow++;
            }
        }
        return slow;
    }
};
```




