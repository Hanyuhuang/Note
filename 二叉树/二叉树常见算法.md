### 二叉树常见算法

#### 二叉树结构定义

```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    public TreeNode(int val) {
      this.val = val;
    }
}
```

#### 二叉树的遍历

##### 前、中、后序遍历(递归)

```java
public void preOrderRecur(TreeNode root) {
    if (root == null) return;
    //前序 System.out.println(root.val);
    preOrderRecur(root.left);
    //中序 System.out.println(root.val);
    preOrderRecur(root.right);
    //后序 System.out.println(root.val);
}
```

##### 前序遍历(非递归)

```java
public void preOrderUnRecur(TreeNode root) {
        if (root == null) return;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            root = stack.pop();
            System.out.println(root.val);
            if (root.right != null) {
                stack.push(root.right);
            }
            if (root.left != null) {
                stack.push(root.left);
            }
        }
    }
```

##### 中序遍历(非递归)

```java
public void inOrderUnRecur(TreeNode root) {
    if (root == null) return;
    Stack<TreeNode> stack = new Stack<>();
    while (root != null || !stack.isEmpty()) {
        if (root != null) {
          stack.push(root);
          root = root.left;
        } else {
          root = stack.pop();
          System.out.println(root.val);
          root = root.right;
        }
    }
}
```

##### 后序遍历(非递归)

```java
// 使用两个栈 模仿前序遍历 修改后的前序遍历出栈为 中后前 按照这个序列入栈 最后出栈为前后中
public void postOrderUnRecur(TreeNode root) {
        if (root == null) return;
        Stack<TreeNode> stack1 = new Stack<>();
        Stack<TreeNode> stack2 = new Stack<>();
        stack1.push(root);
        while (!stack1.isEmpty()) {
            root = stack1.pop();
            stack2.push(root);
            if (root.left != null) {
                stack1.push(root.left);
            }
            if (root.right != null) {
                stack1.push(root.right);
            }
        }
        while (!stack2.isEmpty()) {
            System.out.println(stack2.pop().val);
        }
    }

// 使用一个栈
public void postOrderUnRecur2(TreeNode root) {
        if (root == null) return;
        Stack<TreeNode> stack = new Stack<>();
        Node pre = null;
        stack.push(root);
        while (!stack.isEmpty()){
            root = stack.peek();
            if ((root.left==null&&root.right==null)||
                    (pre!=null&&(pre==root.left||pre==root.right))){
                pre = root;
                System.out.println(stack.pop().val);
            }else{
                if (root.right!=null) stack.push(root.right);
                if (root.left!=null) stack.push(root.left);
            }
        }
    }
```

##### 层次遍历

```java
public void levelOrder(TreeNode root) {
        if (root == null) return;
        Queue<TreeNode> queue = new LinkedList();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            // 遍历每一层
            for (int i = 0; i < size; i++) {
                root = queue.poll();
                System.out.println(root.val);
                if (root.left != null) {
                    queue.offer(root.left);
                }
                if (root.right != null) {
                    queue.offer(root.right);
                }
            }
        }
    }
```

#### 还原二叉树

##### 已知前序、中序遍历序列 还原二叉树

```java
 public TreeNode buildTreeByPreOrderAndInOrder(int[] preorder, int[] inorder) {
        return buildTreeByPreOrderAndInOrder(preorder, inorder, 0, preorder.length - 1, 0, inorder.length - 1);
    }

    private TreeNode buildTreeByPreOrderAndInOrder(int[] preorder, int[] inorder, int preStart, int preEnd, int inStart, int inEnd) {
        if (preStart > preEnd || inStart > inEnd) return null;
        // 根节点为前序遍历第一个
        TreeNode root = new TreeNode(preorder[preStart]);
        // 找到根节点在中序遍历的位置
        int index = 0;
        for (int i = inStart; i <= inEnd; i++) {
            if (inorder[i] == preorder[preStart]) {
                index = i;
                break;
            }
        }
        root.left = buildTreeByPreOrderAndInOrder(preorder, inorder, preStart + 1, preStart + (index - inStart), inStart, index - 1);
        root.right = buildTreeByPreOrderAndInOrder(preorder, inorder, preStart + (index - inStart) + 1, preEnd, index + 1, inEnd);
        return root;
    }
```

##### 已知中序、后序遍历序列 还原二叉树

```java
public TreeNode buildTreeByInorderAndPostOrder(int[] inorder, int[] postorder){
        return buildTreeByInorderAndPostOrder(inorder, postorder, 0, inorder.length - 1, 0, postorder.length - 1);
    }

    private TreeNode buildTreeByInorderAndPostOrder(int[] inorder, int[] postorder, int inStart, int inEnd, int postStart, int postEnd) {
        if (inStart > inEnd || postStart > postEnd) return null;
        // 根节点为后序遍历最后一个
        TreeNode root = new TreeNode(postorder[postEnd]);
        // 得到根节点在中序遍历中的位置
        int index = 0;
        for (int i = inStart; i <= inEnd; i++) {
            if (inorder[i] == postorder[postEnd]) {
                index = i;
                break;
            }
        }
        root.left = buildTreeByInorderAndPostOrder(inorder, postorder, inStart, index - 1, postStart, postStart + (index - inStart) - 1);
        root.right = buildTreeByInorderAndPostOrder(inorder, postorder, index + 1, inEnd, postStart + (index - inStart), postEnd - 1);
        return root;
    }
```

##### 已知前序、后序遍历序列 还原二叉树(不能确定 结果不唯一)

```java
public TreeNode constructFromPrePost(int[] pre, int[] post) {
        return constructFromPrePost(pre, post, 0, pre.length - 1, 0, post.length - 1);
    }

    public TreeNode constructFromPrePost(int[] pre, int[] post, int preStart, int preEnd, int postStart, int postEnd) {
        if (preStart > preEnd || postStart > postEnd) return null;
        // 根节点为前序遍历第一个 或 后序遍历最后一个
        TreeNode root = new TreeNode(pre[preStart]);
        // 数组长度大于1
        if (preEnd - preStart > 0) {
            int index = 0;
            for (int i = postStart; i <= postEnd; i++) {
                if (post[i] == pre[preStart + 1]) {
                    index = i;
                    break;
                }
            }
            root.left = constructFromPrePost(pre, post, preStart + 1, preStart + 1 + index - postStart, postStart, index);
            root.right = constructFromPrePost(pre, post, preStart + 1 + index - postStart + 1, preEnd, index + 1, postEnd - 1);
        }
        return root;
    }
}
```

#### 序列化和反序列化二叉树

```java
     /**
     * 前序遍历 序列化二叉树
     * null节点用 '#' 表示 每个节点用','分隔开
     * @param root
     * @return
     */
    public String Serizalize(TreeNode root) {
        if (root == null) return "#,";
        String s = root.val + ",";
        s += root.left;
        s += root.right;
        return s;
    }

    /**
     * 前序遍历 反序列化
     * 将字符串按','分割开 并加入队列
     * @param string
     * @return
     */
    public TreeNode unSerizalize(String string) {
        String[] strings = string.split(",");
        Queue<String> queue = new LinkedList<>();
        for (String s : strings) {
            queue.offer(s);
        }
        return getNode(queue);
    }

    private TreeNode getNode(Queue<String> queue) {
        String s = queue.poll();
        if (s.equals("#")) return null;
        TreeNode node = new TreeNode(Integer.parseInt(s));
        node.left = getNode(queue);
        node.right = getNode(queue);
        return node;
    }
```

#### 是否是完全二叉树

```java
 public boolean isVaildCBT(TreeNode root) {
        if (root == null) return true;
        Queue<TreeNode> queue = new LinkedList<>();
        boolean leaf = false; //标记此层是否为叶子节点
        TreeNode left = null;
        TreeNode right = null;
        queue.offer(root);
        while (!queue.isEmpty()) {
            root = queue.poll();
            left = root.left;
            right = root.right;
            //   leaf=true 此层右边不能有节点              ||  没有左节点 有右节点
            if (leaf && (left != null || right != null) || (left == null && right != null)) {
                return false;
            }
            if (left != null) {
                queue.offer(left);
            }
            if (right != null) {
                queue.offer(right);
            } else {
                leaf = true;
            }
        }
        return true;
    }
```

#### 完全二叉树的结点个数

```java
		public int getNum(TreeNode root){
        if (root==null) return 0;
        int left = getDepth(root.left);
        int right = getDepth(root.right);
        if (left==right){
            return (1<<left)+getNum(root.right);
        }else {
            return (1<<right)+getNum(root.left);
        }
    }

    private int getDepth(TreeNode root){
        int count = 0;
        while (root!=null){
            count++;
            root = root.left;
        }
        return count;
    }

```

#### 是否为平衡二叉树

```java
 		boolean isBalance = true;

    public boolean isBalancedBinaryTree(TreeNode root){
        getDepth(root);
        return isBalance;
    }

    private int getDepth(TreeNode root){
        if (root==null) return 0;
        int left = getDepth(root.left);
        int right = getDepth(root.right);
        if(Math.abs(left-right)>1) isBalance=false;
        return left>right?left+1:right+1;
    }
```

#### 是否为二叉搜索树

```java
public boolean isValidBST(TreeNode root) {
        if (root == null) return true;
        Stack<TreeNode> stack = new Stack<>();
        TreeNode pre = null;
        while (root != null || !stack.isEmpty()) {
            if (root == null) {
                stack.push(root);
                root = root.left;
            } else {
                root = stack.pop();
                if (pre != null && pre.val >= root.val) return false;
                pre = root;
                root = root.right;
            }
        }
        return true;
    }
```

