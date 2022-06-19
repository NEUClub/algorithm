#### [剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

**分类：**`递归`、`二叉搜索树`

运用递归的方式解决问题：

首先看到的是一个**后续遍历数组** 

![剑指 Offer 33. 二叉搜索树的后序遍历序列.1](http://drawbed.itlearn.club/uPic/%E5%89%91%E6%8C%87%20Offer%2033.%20%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97.1.png)

根据后序遍历的遍历方式：`左——右——根`可以得出当前数组的根节点为 `5`

![剑指 Offer 33. 二叉搜索树的后序遍历序列.2](http://drawbed.itlearn.club/uPic/%E5%89%91%E6%8C%87%20Offer%2033.%20%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97.2.png)

接着我们用一个指针从左向右遍历数组元素，找到第一个比根节点 `5`大的元素，并据此`“左”`和`“右”`的范围

![剑指 Offer 33. 二叉搜索树的后序遍历序列.3](http://drawbed.itlearn.club/uPic/%E5%89%91%E6%8C%87%20Offer%2033.%20%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97.3.png)

此时，递归的基本思想已经梳理完成，为了方便理解，接着向下看：

下面开始对 `132`这个子数组进行递归的操作，步骤同上。

![剑指 Offer 33. 二叉搜索树的后序遍历序列.4](http://drawbed.itlearn.club/uPic/%E5%89%91%E6%8C%87%20Offer%2033.%20%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97.4.png)

最终**没有元素剩余**，**成功拼接成一个二叉搜索树**

![剑指 Offer 33. 二叉搜索树的后序遍历序列.5](http://drawbed.itlearn.club/uPic/%E5%89%91%E6%8C%87%20Offer%2033.%20%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97.5.png)

## 何时不能组成二叉搜索树呢？

*二叉搜索树*的左子树永远是比根节点小的,而它的右子树则都是比根节点大的值。

则如果我们遍历到第一个比根节点大的数字时，若这个数字之后还有`<=`根节点的数字，则不能组成二叉搜索树。

如下图所示：

![剑指 Offer 33. 二叉搜索树的后序遍历序列.6](http://drawbed.itlearn.club/uPic/%E5%89%91%E6%8C%87%20Offer%2033.%20%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97.6.png)

显然根节点 `5`的右子树不可能包含 `2`这种小于等于 `5`的元素。

## 代码实现

```java
class Solution {
     public boolean verifyPostorder(int[] postorder) {
        return isBSTree(postorder, 0, postorder.length - 1);
    }

    private boolean isBSTree(int[] postorder, int left, int right) {
        //（结束条件最后看！）左右指针重合的时候，即left~right区间只有一个数
        if (left >= right) {
            return true;
        }
        //在后序遍历中根节点一定是最后一个点
        int root = postorder[right];
        int index = left;
        while (postorder[index] < root) {
            index++;
        }
        int m = index;//left~right之间第一个比root大的点，即root右子树中最小的点（右子树后序遍历的起点）
        //如果m~right区间（root的右子树）出现了比root小的节点，则不可能是后序遍历
        while (index < right) {
            if (postorder[index] < root) {
                return false;
            }
            index++;
        }
        //此时能保证left ~ m-1都比root小，m ~ right-1都比root大，但这两个子区间内部的情况需要继续递归判断
        return isBSTree(postorder, left, m - 1) && isBSTree(postorder, m, right - 1);
    }
}
```

