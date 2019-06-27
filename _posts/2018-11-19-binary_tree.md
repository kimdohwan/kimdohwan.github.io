---
layout: post
title:  "3. Tree - Bynary Tree"
categories: DataStructure 
---
traversal(순회) 4가지 방식(노드를 방문/접근 시점에 따라)

-   preorder : 탐색 노드 접근 후 접근(root 1번)
-   inorder : left node 순회 후 탐색(가장 왼쪽편 node, left child 없는 node 1번)
-   postorder : 모두 순회 후 접근(가장 왼쪽편 leaf node 1번)
-   levelorder : root 부터 level 에 따라 왼쪽에서 오른쪽으로 탐색(root 1번)

```

class Node:
    def __init__(self, item, left=None, right=None):
        self.item = item
        self.left = left
        self.right = right


class BinaryTree:
    def __init__(self):
        self.root = None

    def preorder(self, n):
        if n != None:
            print(str(n.item), ' ', end='')
            if n.left:
                self.preorder(n.left)
            if n.right:
                self.preorder(n.right)

    def inorder(self, n):
        if n != None:
            if n.left:
                self.inorder(n.left)
            print(str(n.item), ' ', end='')
            if n.right:
                self.inorder(n.right)

    def postorder(self, n):
        if n != None:
            if n.left:
                self.postorder(n.left)
            if n.right:
                self.postorder(n.right)
            print(str(n.item), ' ', end='')

    def levelorder(self, root): # 
        q = []
        q.append(root)
        while len(q) != 0:
            t = q.pop(0)
            print(str(t.item), ' ', end='')
            if t.left != None:
                q.append(t.left)
            if t.right != None:
                q.append(t.right)

    def height(self, root):
        if root == None:
            return 0 
        return max(self.height(root.left), self.height(root.right)) + 1

    # --------여기부터는 책에 없는 코드가 참고자료로 첨부----------
    def size(self, root):
        if root is None:
            return 0
        else:
            return 1 + self.size(root.left) + self.size(root.right)

    def copy_tree(self, n):
        if n == None:
            return None
        else:
            left = self.copy_tree(n.left)
            right = self.copy_tree(n.right)
            return self.Node(n.item, left, right)

    def is_equal(self, n, m):
        if n == None or m == None:
            return n == m
        if n.item != m.item:
            return False
        return (
            self.is_equal(
                n.left, m.left
            ) 
            and 
            self.is_equal(
                n.right, m.right
            )
        )

    # 트리 구조 프린팅해보고 싶어서 만들어 보았다 
    # inorder traveral 이 적합한듯하여 이용
    def print_tree(self, root):    
        if root.left:
            self.print_tree(root.left)
        print('---|' * self.level(root), root.item)
        if root.right:
            self.print_tree(root.right)

    # height 는 root 를 인자로 넣을 경우, 높이 최댓값이 나옴
    # 나는 leaf 노드에 높이 최댓값을 넣기위해(가장 왼쪽편에 root node 놓기 위해서)
    # heifgt 함수를 참고해서 바꿔보았다
    # height 를 사용하지 않고 아예 레벨만 구현 할 수 있을거같은데 복습 때 해보자
    def level(self, root):
        if root == None: # leaf node 에 접근한다면
            return self.height(self.root) # tree 의 height 값(ex: 3)

        # leaf node 라면 return min(height, height) -1 로 리턴된다
        # child 가 한개만 있는 node 라면 return min(height, -1 누적 값) -1 
        # 따라서 min 으로 -1 누적값을 취해야한다(max가 아니라)
        return min(self.level(root.left), self.level(root.right)) - 1
```