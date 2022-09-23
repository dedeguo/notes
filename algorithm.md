# 算法

- [x] this is a complete item
- [ ] this is an incomplete item


 按照出现频率顺序：
 - [x] [1. 两数之和](#两数之和)
 - [x] [5. 最长回文子串](#最长回文子串)
 - [x] [3. 无重复字符的最长子串](#无重复字符的最长子串)
 - [ ] [15. 三数之和](#15-三数之和)
-  [x] [217. 存在重复元素](#217-存在重复元素)
### 324.摆动排序

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
### 526. 优美的排列
回溯法
```java
class Solution {
     int count = 0;
        //回溯法
    public int countArrangement(int n) {

        //map consist of <num,visited>
        Map<Integer,Integer> map = new HashMap<>();
        for (int i=1;i<=n;i++){
            map.put(i,0);
        }
        backTrace(map,1,n);
        return count;
    }

    private void backTrace(Map<Integer,Integer> map, int i,int n){
       for (Integer num : map.keySet()){
           Integer v = map.get(num);
           if (v==0 && (num%i==0  || i%num==0)){
               map.put(num,1);
               if (i==n) count++;
               else {
                   backTrace(map,i+1,n);
               }
               map.put(num,0);
           }
       }
    }

}
```

### 两数之和
1 暴力2轮循环，时间复杂度$n^2$
```java
public int[] twoSum(int[] nums, int target) {

        int[] res=new int[2];
        for (int i=0;i<nums.length-1;i++){
            for (int j=i+1;j<nums.length;j++){
                if (nums[i]+nums[j]==target){
                    res[0]=i;
                    res[1]=j;
                    return res;
                }
            }
        }
        return res;
    }
```
2.优化查找 第2层查找使用二分查找，复杂度 $n\log(n)$
3. 使用hash表优化查找
```java
public int[] twoSum(int[] nums, int target) {
            Map<Integer, Integer> hash = new HashMap<>();
            for (int i = 0; i < nums.length; i++) {
             if (hash.containsKey(target-nums[i]))
                 return new int[]{i,hash.get(target-nums[i])};
             else {
                 hash.put(nums[i],i);
             }
            }
            return null;
        }
```

### 最长回文子串

```java

     //中心扩展法
    public String longestPalindrome(String s) {
        if(s==null || s.length()==0) return "";
        int start = 0, end = 0;
        for (int i=0;i<s.length();i++){
            int len1=expandFromCenter(i,i,s);
            int len2=expandFromCenter(i,i+1,s);
            int temStart, temEnd;
            if (len1>len2){
               temStart=i-(len1-1)/2;
               temEnd = i+(len1-1)/2;
            }else {
                temStart=i-(len2-2)/2;
                temEnd=i+len2/2;
            }
            if (temEnd-temStart > end-start){
                start=temStart;
                end=temEnd;
            }
        }
        return s.substring(start,end+1);
    }

    int expandFromCenter(int left, int right, String s){
        while (left>=0 && right<s.length()
                && s.charAt(left)==s.charAt(right) ){
            left--;
            right++;
        }
        //边界情况
        return right-left-1;
    }

```

### 无重复字符的最长子串

```java
/**
 * 1.暴力枚举所有子串超时
 * 2.滑动窗口
 */
 public int lengthOfLongestSubstring(String s) {
            int begin=0;//窗口起始位置
            int maxLen=0;
            Map<Character,Integer> map=new HashMap<>();
            for (int end=0;end<s.length();end++){
                if (!map.containsKey(s.charAt(end))  ||
                        (map.containsKey(s.charAt(end))) && map.get(s.charAt(end))<begin){
                    maxLen=Math.max(maxLen,end-begin+1);
                    map.put(s.charAt(end),end);
                } else {
                    begin = map.get(s.charAt(end))+1;
                    map.put(s.charAt(end),end);
                }
            }
            return maxLen;
        }


```


### 15. 三数之和
```java
//两数之和的升级版，暴力解决法 todo


/**
排序+双指针
**/
public List<List<Integer>> threeSum(int[] nums) {
            List<List<Integer>> result = new ArrayList<>();
            Arrays.sort(nums);
            for (int i = 0; i < nums.length; i++) {
                if (nums[i] > 0) return result;
                if (i > 0 && nums[i] == nums[i - 1]) continue;
                int left = i + 1;
                int right = nums.length - 1;
                while (right > left) {
                    if (nums[i] + nums[left] + nums[right] == 0) {
                        List<Integer> list = new ArrayList<Integer>();
                        list.add(nums[i]);
                        list.add(nums[left]);
                        list.add(nums[right]);
                        result.add(list);
                        while (left < right && nums[left] == nums[left + 1])
                            left = left + 1;
                        while (left < right && nums[right] == nums[right - 1])
                            right = right - 1;
                        left++;
                        right--;
                    } else if (nums[i] + nums[left] + nums[right] > 0) {
                        right--;
                    } else {
                        left++;
                    }
                }
            }
            return result;
        }

```


### 217. 存在重复元素
```java
/**
1.hash
2.sort
**/
        public boolean containsDuplicate(int[] nums) {
            Arrays.sort(nums);
            if (nums.length==1) return false;
            for (int i=1;i<nums.length;i++){
                if (nums[i-1]==nums[i]) return true;
            }
            return false;
        }
        public boolean containsDuplicateHash(int[] nums) {
            HashSet<Integer> data = new HashSet<>();
            for (int num : nums) {
                if (!data.contains(num))
                    data.add(num);
                else {
                    return false;
                }
            }
            return true;
        }

```