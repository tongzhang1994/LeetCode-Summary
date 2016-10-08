# Backtracking

> [Here are summary of some `classic Backtracking` problems](https://github.com/TongZhangUSC/LeetCode-Summary/blob/master/Classic%20Backtrackings.md)

The followings are backtracking problem with more than one solution or with a few tricky points in it.

## [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

I was really inspired by [this post](http://www.1point3acres.com/bbs/forum.php?mod=redirect&goto=findpost&ptid=172641&pid=2237150&fromuid=96813). That guy summarized a general idea of `Backtracking`. I try to translate into English and add some supplemental materials.

*WIKIPEDIA: Backtracking is a general algorithm for finding all (or some) solutions to some computational problems that incrementally builds candidates to the solutions, and abandons each partial candidate c ("backtracks") as soon as it determines that c cannot possibly be completed to a valid solution.*

>The basic thought is that: You have many options under current senario and choose one of them each time. You must fall into the following two cases-either you find this option, violating some restraints, is invalid and abandon it or it is valid in the end and you add it to the solution.
>
>So, whenever you are thinking about `backtracking`, you only need to consider three things: OPTIONS, RESTRAINTS and TERMINATION.

In this problem, you always have two options:

1. add left parenthese
2. add left parenthese

And you always have these restraints:

1. if the number of left parentheses is equal to n, then you can't add left parentheses any more.
2. if the number of left parentheses is equal to number of right parentheses, then you can't add right parentheses any more. 

The termination is:
All left and right parentheses are used up. This must be the correct solution and you can add it to the result set.

Here is the code.

```java
	public List<String> generateParenthesis(int n) {
        int left=0,right=0;
        List<String> res=new ArrayList<>();
        backtracking(res,"",0,0,n);
        return res;
    }
    private void backtracking(List<String> res,String p,int left,int right,int n){
        if(left==n&&right==n){
            res.add(p);
            return;
        }
        if(left<n)  backtracking(res,p+"(",left+1,right,n);
        if(right<left) backtracking(res,p+")",left,right+1,n);
    }
```


## [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

**Solution1: Backtracking(DFS)**

Based on the previous generalization, we can come up with the ORT(OPTIONS,RESTRAINTS,TERMINATION) for this problem.

OPTION:
choose the next charactor in the mapping of next digit.

RESTRAINT:
if all charactors in the mapping charactors of certain digits are used up , then you have to stop this level of recursion and back to previous level.

TERMINATION:
if all digits have been considered, then you get the final result.

```java
	public List<String> letterCombinations(String digits) {
        List<String> res=new ArrayList<>();
        if(digits.length()==0)  return res;
        String[] map={"0","1","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
        backtracking(res,digits,map,"",0);
        return res;
    }
    private void backtracking(List<String> res,String digits,String[] map,String s,int start){
        if(start==digits.length()){
            res.add(s);
            return;
        }
        for(int i=0;i<map[digits.charAt(start)-'0'].length();i++){
            backtracking(res,digits,map,s+map[digits.charAt(start)-'0'].charAt(i),start+1);
        }
    }
```

**Solution2:FIFO queue(BFS)**

This is a iterative solution. For each digit added, remove and copy every element in the queue and add the possible letter to each element, then add the updated elements back into queue again. Repeat this procedure until all the digits are iterated.

```java
	public List<String> letterCombinations(String digits) {
        LinkedList<String> res=new LinkedList<>();//List会返回peek未定义
        if(digits.length()==0)  return res;
        String[] map={"0","1","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
        res.add("");
        for(int i=0;i<digits.length();i++){
            int d=digits.charAt(i)-'0';
            while(res.peek().length()==i){
                String tmp=res.remove();
                for(char c:map[d].toCharArray())
                    res.add(tmp+c);
            }
        }
        return res;
    }
```

## [Binary Watch](https://leetcode.com/problems/binary-watch/)

**Solution1: BitCount**

```java
	public List<String> readBinaryWatch(int num) {
        //countbit
        List<String> res=new ArrayList<>();
        for(int h=0;h<12;h++)
            for(int m=0;m<60;m++)
                if(Integer.bitCount(h*64+m)==num)//h+m*16
                    res.add(String.format("%d:%02d",h,m));
        return res;
    }
```

**Solution2:Backtracking**

OPTION:
choose the next charactor in the mapping of next digit.

RESTRAINT:
if all charactors in the mapping charactors of certain digits are used up , then you have to stop this level of recursion and back to previous level.

TERMINATION:
if all digits have been considered, then you get the final result.

```java
	public List<String> readBinaryWatch(int num) {
        List<String> res=new ArrayList<String>();
        int[] bits1={1,2,4,8},bits2={1,2,4,8,16,32};
        for(int i=0;i<=num;i++){
            List<Integer> hours=helper(bits1,i);
            List<Integer> mins=helper(bits2,num-i);
            for(Integer h:hours){
                if(h>=12)    continue;
                for(Integer m:mins){
                    if(m>=60)    continue;
                    res.add(h+":"+(m<10?"0"+m:m));
                }
            }
        }
        return res;
    }
    private List<Integer> helper(int[] bits,int n){
        List<Integer> res1=new ArrayList<>();
        backtracking(res1,bits,n,0,0);
        return res1;
    }
    private void backtracking(List<Integer> res1,int[] bits,int n,int start,int sum){
        if(n==0){
            res1.add(sum);
            return;
        } 
        for(int i=start;i<bits.length;i++)
            backtracking(res1,bits,n-1,i+1,sum+bits[i]);
    }
```


## [Word Search](https://leetcode.com/problems/word-search/)

This problem seems tricky, but it is easy if you've understand it. Just go through the board, check if each board element match the first charactor of String `s`, and recursively call backtracking method to check its ajacent cells. The only different thing you need to do is to store the state(visited or not visited) of each grid since `The same letter cell may not be used more than once.`.

```java
	public boolean exist(char[][] board, String word) {
        for(int i=0;i<board.length;i++)
            for(int j=0;j<board[0].length;j++)
                if(backtracking(board,word,i,j,0))   return true;
        return false;
    }
    private boolean backtracking(char[][] board,String word,int i,int j,int idx){
        if(idx==word.length())    return true;
        if(i<0||j<0||i==board.length||j==board[0].length)   return false;
        if(board[i][j]!=word.charAt(idx))   return false;
        board[i][j]^=256;//bit mask->invalid char
        boolean exist=backtracking(board,word,i+1,j,idx+1)||backtracking(board,word,i,j+1,idx+1)||backtracking(board,word,i-1,j,idx+1)||backtracking(board,word,i,j-1,idx+1);
        board[i][j]^=256;//bit mask->valid char
        return exist;
    }
```
