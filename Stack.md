# Stack

## [Mini Parser](https://leetcode.com/problems/mini-parser/)

I think the most important point of this problem is to read the instruction carefully and make sure you've understood it.

Here is part of the instruction:

> **Example 1:** Given s = "324",
>
> You should return a NestedInteger object which contains a single integer 324.
> 
> **Example 2:**
> Given s = "[123,[456,[789]]]",
> 
> Return a NestedInteger object containing a nested list with 2 elements:
>
> &nbsp;&nbsp;&nbsp;An integer containing value 123.
> 
> &nbsp;&nbsp;&nbsp;A nested list containing two elements:
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i.  An integer containing value 456.
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ii. A nested list with one element:
> 
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;An integer containing value 789.

Take a careful look at these two examples, you can find that:

iterate through the input string,

whenever we encounter a `"["`, create a list and push it into stack. Then set the `start` point to i+1.

whenever we encounter a  `","` or `"]"`, 

&nbsp;&nbsp;&nbsp;if `i<start`, meaning we've reached the end of an integer not yet added to the list, create a new NestedInteger and append it to current list. Then push it into stack.

&nbsp;&nbsp;&nbsp;Set the new `start` point to i+1.

&nbsp;&nbsp;&nbsp;if it is a `"]"`, it also means an ending of a list. So we pop the list from the stack to ensure the future append operation is done in the correct place.

```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Set this NestedInteger to hold a single integer.
 *     public void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
	public NestedInteger deserialize(String s) {
        if(!s.startsWith("["))
            return new NestedInteger(Integer.valueOf(s));
        Stack<NestedInteger> stack=new Stack<>();
        NestedInteger res=new NestedInteger();
        stack.push(res);//push empty 'res' list to stack
        int start=1;//to keep track of the next begin index of a new single NestedInteger
        for(int i=1;i<s.length();i++){
            if(s.charAt(i)=='['){//meaning the begining of a new list
                NestedInteger ni=new NestedInteger();
                stack.peek().add(ni);//append this new empty list to the current list(peek) in stack
                stack.push(ni);//push this new empty list to the stack peek for purpose of furture use(append new list)
                start=i+1;//set new beginning of a list of single NestedInteger
            }else if(s.charAt(i)==']'||s.charAt(i)==','){//meaning the end of a list or single NestedInteger
                if(start<i){//meaning there is a single NestedInteger which hasn't been added to the 'res' list
                    NestedInteger ni=new NestedInteger(Integer.valueOf(s.substring(start,i)));
                    stack.peek().add(ni);//append single NestedInteger to the stack peek(must be a previously added list)
                }
                start=i+1;
                if(s.charAt(i)==']'){
                    stack.pop();
                }
            }
        }
        return res;
    }
```

P.S. Here is an implementation of NestedInteger Class written by [fishercoder](https://discuss.leetcode.com/topic/54268/straightforward-java-solution-with-explanation-and-a-simple-implementation-of-nestedinteger-for-your-ease-of-testing/2)

```java
class NestedInteger {
    private List<NestedInteger> list;
    private Integer integer;
    
    public NestedInteger(List<NestedInteger> list){
        this.list = list;
    }
    
    public void add(NestedInteger nestedInteger) {
        if(this.list != null){
            this.list.add(nestedInteger);
        } else {
            this.list = new ArrayList();
            this.list.add(nestedInteger);
        }
    }

    public void setInteger(int num) {
        this.integer = num;
    }

    public NestedInteger(Integer integer){
        this.integer = integer;
    }

    public NestedInteger() {
        this.list = new ArrayList();
    }

    public boolean isInteger() {
        return integer != null;
    }

    public Integer getInteger() {
        return integer;
    }

    public List<NestedInteger> getList() {
        return list;
    }
    
    public String printNi(NestedInteger thisNi, StringBuilder sb){
        if(thisNi.isInteger()) {
            sb.append(thisNi.integer);
            sb.append(",");
        }
        sb.append("[");
        for(NestedInteger ni : thisNi.list){
            if(ni.isInteger()) {
                sb.append(ni.integer);
                sb.append(",");
            }
            else {
                printNi(ni, sb);
            }
        }
        sb.append("]");
        return sb.toString();
    }
}
```
