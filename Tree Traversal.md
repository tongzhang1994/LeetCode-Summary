# Tree Traversal

## [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

**Preorder**

root->left subtree->right subtree

**1.recursion**

```java
	public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res=new ArrayList<>();
        if(root==null)  return res;
        res.add(root.val);
        res.addAll(preorderTraversal(root.left));
        res.addAll(preorderTraversal(root.right));
        return res;
    }
```

**2.iteration**

```java
	public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res=new LinkedList<>();
        Stack<TreeNode> stack=new Stack<>();
        stack.push(root);
        while(!stack.empty()){
            TreeNode node=stack.pop();
            if(node!=null){
                res.add(node.val);
                stack.push(node.right);
                stack.push(node.left);
            }
        }
        return res;
    }
```

## [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

**1.recursion**

```java
	public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res=new ArrayList<>();
        if(root==null)  return res;
        res.addAll(inorderTraversal(root.left));
        root.add(root.val);
        root.addAll(inorderTraversal(root.right));
        return res;
    }
```

**2.iteration**

```java
	public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res=new LinkedList<>();
        Stack<TreeNode> stack=new Stack<>();
        while(root!=null||!stack.empty()){
            while(root!=null){
                stack.push(root);
                root=root.left;
            }
            root=stack.pop();
            res.add(root.val);
            root=root.right;
        }
        return res;
	}
```

## [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

**1.iteration**

```java
	public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<Integer>();
        Stack<TreeNode> stack=new Stack<TreeNode>();
        TreeNode prev=null;
        while(root!=null||!stack.empty())
            if(root!=null){
                stack.push(root);
                root=root.left;
            }else{
                TreeNode tmp=stack.peek();
                if(tmp.right!=null&&tmp.right!=prev)    root=tmp.right;
                else{
                    stack.pop();
                    res.add(tmp.val);
                    prev=tmp;
                }
            }
        return res;
    }
```

**2.recursion**

```java
	public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<Integer>();
        if(root==null)  return res;
        res.addAll(postorderTraversal(root.left));
        res.addAll(postorderTraversal(root.right));
        res.add(root.val);
        return res;
    }
```

## [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

```java
	public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res=new ArrayList<>();
        Queue<TreeNode> queue=new LinkedList<>();
        if(root==null)  return res;
        queue.offer(root);
        while(!queue.isEmpty()){
            List<Integer> list=new ArrayList<>();
            int size=queue.size();
            for(int i=0;i<size;i++){
                TreeNode node=queue.poll();
                list.add(node.val);
                if(node.left!=null)     queue.offer(node.left);
                if(node.right!=null)    queue.offer(node.right);
            }
            res.add(new ArrayList<Integer>(list));
        }
        return res;
    }
```

## [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

```java
	public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res=new ArrayList<>();
        Queue<TreeNode> queue=new LinkedList<>();
        if(root==null)  return res;
        queue.add(root);
        int level=1;
        while(!queue.isEmpty()){
            int size=queue.size();//necessary, cause queue size would be modified later.
            List<Integer> list=new ArrayList<>();
            for(int i=0;i<size;i++){
                TreeNode node=queue.poll();
                if(level%2!=0)  list.add(node.val);
                else    list.add(0,node.val);
                if(node.left!=null)     queue.add(node.left);
                if(node.right!=null)    queue.add(node.right);
            }
            level++;
            res.add(new ArrayList<>(list));
        }
        return res;
    }
```

