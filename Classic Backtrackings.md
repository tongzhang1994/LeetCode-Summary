# Classic Backtrackings

Here I list all classic `Backtracking` problems with almost identical solution form. Try your best to understand the codes.

## [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

I think this is also a classic problem. This only difference is to deal with palindrome. 

>Please refer to the `recursion tree` in [this post](https://discuss.leetcode.com/topic/6186/java-backtracking-solution) in Leetcode discussion since this might help you understand most backtracking problems.

OPTION: 
add a substring to 'palindrome list'

RESTRAINT:
the substring must be a palindrome.=>You have to check it.

TERMINATION:
as long as you have gone through the whole input string, you finished an iteration, return and go on the next one.

```java
	public List<List<String>> partition(String s) {
        List<List<String>> res=new ArrayList<List<String>>();
        backtracking(res,new ArrayList<String>(),s,0);
        return res;
    }
    private void backtracking(List<List<String>> res, List<String> list, String s, int start){
        if(start==s.length()){
            res.add(new ArrayList<String>(list));
            return;
        }
        for(int i=start;i<s.length();i++)
            if(isPalindrome(s,start,i)){
                list.add(s.substring(start,i+1));
                backtracking(res,list,s,i+1);
                list.remove(list.size()-1);
            }
    }
    private boolean isPalindrome(String s, int i, int j){
        while(i<j)
            if(s.charAt(i++)!=s.charAt(j--))    return false;
        return true;
    }
```


## [Combinations](https://leetcode.com/problems/combinations/)

```java
	public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res=new ArrayList<List<Integer>>();
        backtracking(res,new ArrayList<>(),1,n,k);
        return res;
    }
    private void backtracking(List<List<Integer>> res,List<Integer> comb,int start,int n,int k){
        if(comb.size()==k){
            res.add(new ArrayList<>(comb));
            return;
        }
        for(int i=start;i<=n;i++){
            comb.add(i);
            backtracking(res,comb,i+1,n,k);
            comb.remove(comb.size()-1);
        }
    }
```

## [Combination Sum](https://leetcode.com/problems/combination-sum/)

```java
	public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res=new ArrayList<List<Integer>>();
        backtracking(candidates,res,new ArrayList<Integer>(),target,0);
        return res;
    }
    private void backtracking(int[] candidates,List<List<Integer>> res,List<Integer> tmp,int sum,int start){
        if(sum==0)    res.add(new ArrayList<Integer>(tmp));
        for(int i=start;i<candidates.length;i++)
            if(candidates[i]<=sum){
                tmp.add(candidates[i]);
                backtracking(candidates,res,tmp,sum-candidates[i],i);//i:the same number may be reused
                tmp.remove(tmp.size()-1);
            }
    }
```

## [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

```java
	public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> res=new ArrayList<List<Integer>>();
        backtracking(candidates,res,new ArrayList<Integer>(),target,0);
        return res;
    }
    private void backtracking(int[] candidates,List<List<Integer>> res,List<Integer> tmp,int sum,int start){
        if(sum==0)    res.add(new ArrayList<Integer>(tmp));
        for(int i=start;i<candidates.length;i++){
            if(i>start&&candidates[i]==candidates[i-1])  continue;
            if(candidates[i]<=sum){
                tmp.add(candidates[i]);
                backtracking(candidates,res,tmp,sum-candidates[i],i+1);
                tmp.remove(tmp.size()-1);
            }
        }
    }
```

## [Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)

```java
	public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        backtracking(res, new ArrayList<Integer>(),n, k, 1);
        return res;
    }
    private void backtracking(List<List<Integer>> res,List<Integer> tmp,int n,int k,int start){
        if(n==0&&tmp.size()==k){
            res.add(new ArrayList<Integer>(tmp));//no return
            return;
        }
        for(int i=start;i<=9;i++){
            tmp.add(i);
            backtracking(res,tmp,n-i,k,i+1);//why not i+1??
            tmp.remove(tmp.size()-1);
        }
    }
```

## [Permutation](https://leetcode.com/problems/permutations/)

In this problem, we output all permutations of the array. We don't need to store the `start` value in each recursion since we start at position 0 every time and add only if there is no such element in `tmp` list.

```java
	public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res=new ArrayList<List<Integer>>();
        backtracking(res,new ArrayList<Integer>(),nums);
        return res;
    }
    private void backtracking(List<List<Integer>> res,List<Integer> tmp,int[] nums){
        if(tmp.size()==nums.length){
            res.add(new ArrayList<>(tmp));
            return;
        }
        for(int i=0;i<nums.length;i++){
            if(tmp.contains(nums[i]))   continue;
            tmp.add(nums[i]);
            backtracking(res,tmp,nums);
            tmp.remove(tmp.size()-1);
        }
    }
```

## [Permutation II](https://leetcode.com/problems/permutations-ii/)

The difference between this problem and `Permutation` is that there are duplicate elements in the input array. Additional work we should do is to skip those duplicate elements **after the `1st` recursion**. 

So, how to skip? Here we use a boolean array called `added` to keep track of which element has been added to the `tmp` list. Consider two elements with identical value, if the former one hasn't been added to `tmp`, then adding the later one to `tmp` would reault in duplicate subsets in `res` list. (Since we are visting the elements in order, at the moment we visit the later element, all valid subsets containing the former one **at the current condition** must have already been added to `res` list). Thus we should skip the later duplicate element when its former one has not been added to `tmp`. 

That is to say, we only add the later duplicate element when the former one has been added to `tmp`.

```java
	public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res=new ArrayList<List<Integer>>();
        Arrays.sort(nums);
        boolean[] added=new boolean[nums.length];
        backtracking(res,new ArrayList<Integer>(),nums,used);
        return res;
    }
    private void backtracking(List<List<Integer>> res,List<Integer> tmp,int[] nums,boolean[] added){
        if(tmp.size()==nums.length){
            res.add(new ArrayList<>(tmp));
            return;
        }
        for(int i=0;i<nums.length;i++){
            if(added[i]||i>0&&nums[i]==nums[i-1]&&!added[i-1])   continue;
            tmp.add(nums[i]);
            added[i]=true;
            backtracking(res,tmp,nums,added);
            tmp.remove(tmp.size()-1);
            added[i]=false;
        }
    }
```

## [Subsets](https://leetcode.com/problems/subsets/)

**Solution1: Bit Manipulation**

> This solution directly captures the intrinsic connection between power set and binary numbers.
When forming a subset, for each element, only 2 possiblities, either it is in the subset or not in the subset, hence we have total number of possible subsets = 2^n.Thinking each element as a bit, it's either on or off when forming the ith subset, and that's the solution!

```java
	public List<List<Integer>> subsets(int[] nums) {
        //bit manipulation
        List<List<Integer>> res=new ArrayList<List<Integer>>();
        int setsNum=1<<nums.length;//2^nums.length
        for(int i=0;i<setsNum;i++){
            List<Integer> subset=new ArrayList<>();
            for(int j=0;j<nums.length;j++)
                if((i&(1<<j))!=0)//pay 
                    subset.add(nums[j]);
            res.add(subset);
        }
        return res;
    }
```

**Solution2: Backtracking**

This problem asks us to output all subsets. Every time we add a new valid element into `subset`, it forms a valid 'subset'. This means that we have to add the `subset` list in every recursion.

```java
	public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res=new ArrayList<List<Integer>>();
        backtracking(nums,res,new ArrayList<Integer>(),0);
        return res;
    }
    //[] [1] [1,2] [1,2,3] [1,3] [2] [2,3] [3]
    private void backtracking(int[] nums,List<List<Integer>> res,List<Integer> subset,int start){
        res.add(new ArrayList<Integer>(subset));
        for(int i=start;i<nums.length;i++){
            subset.add(nums[i]);
            backtracking(nums,res,subset,i+1);
            subset.remove(subset.size()-1);
        }
    }
```

## [SubsetsII](https://leetcode.com/problems/subsets-ii/)

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> res=new ArrayList<List<Integer>>();
        Arrays.sort(nums);
        backtracking(nums,res,new ArrayList<Integer>(),0);
        return res;
    }
    
    private void backtracking(int[] nums,List<List<Integer>> res,List<Integer> subset,int start){
        res.add(new ArrayList<Integer>(subset));
        for(int i=start;i<nums.length;i++){
            if(i>start&&nums[i]==nums[i-1]) continue;
            subset.add(nums[i]);
            backtracking(nums,res,subset,i+1);
            subset.remove(subset.size()-1);
        }
    }
```

