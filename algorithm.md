# 刷题（防止老年痴呆）

//todo：
- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item
- [ ] this is an incomplete item

[MarkDown方式的锚点](#325.摆动排序)


## 324.摆动排序

```java
/**
先整体排序，在分组。但存在部分相等. 
通过测试用例：47 / 52
用例[4,5,5,6] 不通过。

**/
public void wiggleSort(int[] nums) {
      Arrays.sort(nums);
        int lagerNum=nums.length/2;
        int smallerNum=nums.length-lagerNum;
        int[] sorted=new int[nums.length];
        int  i=0;
        int j=smallerNum;
        int pos=0;
        while (j<nums.length){
            sorted[pos++]=nums[i++];
            sorted[pos++]=nums[j++];
        }
        if (nums.length%2!=0){
            sorted[pos]=nums[i];
        }
        for (int k=0;k<nums.length;k++){
            nums[k]=sorted[k];
        }
}
```

### 606. Construct String from Binary Tree 根据二叉树创建字符串

```java
class Solution {
    public String tree2str(TreeNode root) {


        if (root!=null){
            // left and right is not null
            if (root.left!=null && root.right!=null){
                return root.val+"("+tree2str(root.left)+")("+tree2str(root.right)+")";
            }else if (root.left!=null && root.right==null){
                return root.val+"("+tree2str(root.left)+")";
            }else if (root.left==null && root.right!=null){
                return root.val+"()("+tree2str(root.right)+")";
            }else
                return String.valueOf(root.val);
        }
       return "";
    }

}
```