---

title:  "4. Search Tree - Bynary Search Tree"
tag: DataStructure 
---
binary search(이진탐색, 미리 정렬되어 있어야 한다)  
quick sort 에서 pivot 을 기준으로 둘씩 쪼개가는 방식의 원형인듯  
이걸 먼저 공부하고 quick sort 를 봤다면 이해가 더 쉬웠을 것 같다.  
배열을 둘씩 쪼개가는 방식은 logN 의 시간이 소요

```

class Node:
    def __init__(self, key, value, left=None, right=None):
        self.key = key # index 에 해당하는 부분
        self.value = value
        self.left = left
        self.right = right

class BinarySearchTree:
    def __init__(self):
        self.root = None

    def print_tree(self, root):
        if root.left:
            self.print_tree(root.left)
        print(root.key, root.value, end=' | ')
        if root.right:
            self.print_tree(root.right)

    # 사용자는 get 을 통해 찾고자하는 k(key, index) 만 입력
    # get 함수는 root Node 와 k 를 인자로 넘겨서 search 실행
    def get(self, key): 
        return self.get_item(self.root, k)

    def get_item(self, n, k):
        if n == None: # n=root 이므로 root 없으면 None
            return None
        if n.key > k: # 트리 왼쪽 sub tree로 이동
            return self.get_item(n.left, k)
        if n.key < k: # 트리 오른쪽 sub tree 로 이동
            return self.get_item(n.right, k)
        if n.key == k: # key 값 일치 시 return value
            return n.value

    def put(self, key, value): # item 을 insert 하고 self.root 새로 설정
        # root 가 없다면,
        # 새로 생성된 Node 를 self.root 로 설정
        # root 가 있다면,
        # put_item() 에 넣은 self.root 이 다시 설정됨(결국 똑같이 유지된다)
        self.root = self.put_item(self.root, key, value) 

    # 이 함수는 self.root 가 될 Node를 return    
    def put_item(self, n, key, value):
        # Node 없을 경우, 새 Node 생성해서 return
        if n == None:
            return Node(key, value)

        if n.key > key:
        # n.left 가 None 일 경우:
        # 생성된 Node 를 left 에 설정해주고 밑으로 내려가서 
        # n(생성된 Node 의 parent) return
        # n.left 가 None 이 아닐경우(이미 존재):
        # 재귀적으로 탐색 하여 self.put_item 은 n.left 를 return 하게된다.
        # n.left 가 새로 설정되는 듯하지만 결국 같은 값을 또 넣어주게 되는 과정이다.
            n.left = self.put_item(n.left, key, value) # 
        elif n.key < key:
            n.right = self.put_item(n.right, key, value)
        else:
            # value 값만 새로 업데이트 해준다(put = insert+modify,update)
            n.value = value 

        # 최하단 스택에서는 self.root 값을 리턴
        # 최하단이 아닌(재귀로 쌓인) 스택에서는 n.left or n.right 를 return
        return n 

    def search_min(self):
        if self.root == None:
            return None
        return self.minimum(self.root) # root 말고 다른 인자 필요없다.

    # 이진트리구조에서 최소값은 가장 왼쪽에 위치
    # 최소값: value 를 의미
    # key 와 value 를 헷갈리지말자 
    # 이진트리의 경우 아래나 위에서 봤을 때 왼쪽에서 오른쪽으로 value 가 정렬되어있다.
    # 이 때 정렬의 기준이 되는 값은 key 가 아닌 value 이다?????
    # 하지만 key 와 value 의 정렬순서는 결국 똑같다????
    # 계속 생각해보면서 개념을 잡자 
    def minimum(self, n):
        if n.left == None:
            return n
        return self.minimum(self, n.left)

    def delete_min(self):
        if self.root == None:
            print('empty Tree')
        self.root = self.del_min(self.root)

    # 실행되는 기본적인 구조는 search_min(), minimum() 과 같다
    # 그런데 del_min 의 구조를 이해하는데 오랜 시간이 걸렸다.
    # 왜냐하면 최소값을 삭제 후 '연결' 하는 과정의 재귀연산에 낯설었기 때문이다.
    # 이 코드를 볼 때는 '연결'해줘야 한다는 점에 point 를 두고 공부하자.
    # 이진트리에서 삭제란 '노드간의 연결 교체(left,right)' 일 뿐이다.
    def del_min(self, n):
        if n.left == None: # n 이 최소값에 해당(더이상 left 가 없으므로)
            # right 는 밑 줄을 통해 새로 parent.left 로 설정된다.
            # right 도 None 일 경우, 
            # 현재 n 의 parent.left 는 None 으로 설정되고
            # return n(parent) 이 실행된다.
            return n.right 
        # self.del_min(n.left) 의 
        # n.left가 최소값일 경우(n.left.left == None):
        # n.left = n.left.right 로 다시 설정된다.
        # n.left가 최소값이 아닐 경우(n.left.left 가 존재):
        # n.left = n.left 로 기존의 값을 다시 재설정할뿐이다.
        n.left = self.del_min(n.left)
        return n

    def delete(self, k):
        self.root = self.del_node(self.root, k)

    def del_node(self, n, k):
        if n == None:
            return None

        # key 값을 찾아 내려가는 과정(자리 찾기)
        # 크기(<,>) 로 걸러내면 남는건 n.key == k 인 경우
        if n.key > k:
            n.left = self.del_node(n.left, k)
        elif n.key < k:
            n.right = self.del_node(n.right, k)

        # 이제 n.key == k 인 경우 삭제(연결 조정) 실행
        else:
            # child Node 가 있니? 없으면 넌 삭제될거니까 
            # 삭제될 자리에 들어갈 child 를 return 해주렴
            # 만약 left 나 right 가 있다면 맨밑 return n으로 갈거야 
            # 검색하려는 항목 k 의 자식노드를 검사하는 항목이라고 볼 수 있지
            # 만약 child 가 없는놈이라면 
            if n.right == None:
                return n.left
            if n.left == None:
                return n.right
# -----------여기까지 target k 의 child 가 0 or 1 인 경우-------------

# -----------여기서부터는 child 가 2개 모두 있는 경우 ------------------            
            target = n
            # 삭제될 n 자리에 n.right child 중 가장 작은놈(left로 타고가서)을
            # 지정해줄거야
            # n.right 에서 가장 작은놈을 대입하던, n.left 에서 가장 큰놈을 대입하던
            # 트리의 구조는 틀리지 않아. 2가지 경우가 모두 만족하는거지
            # 하지만 우리가 여기서 n.right 의 가장 작은놈을 택하는 이유는 
            # 우리에게 min 값을 검색하고, 삭제할 수 있는 함수를 미리 만들었기때문이야
            # 만약 max 함수를 만들었다면 n.left 의 max 값을 대입해도 되겠지 
            # 이부분에서 왜 right 인지 고민하느라 시간이 오래걸렸지만 풀어내서 다행.
            n = self.minimum(target.right) 
            # 위쪽에서 우리는 타겟 k 를 찾아냈고
            # k 에 적당한 값을 넣어줬어(k.right child 중 최소값)
            # 그렇다면 n 에 대입된 k.right(min child)를 없애줘야해
            # 지금은 두개의 같은 값이 트리에 존재하거든 
            # 최소값 삭제를 위해 del_min 함수를 쓸거고 del_min 을 통해 
            # return 되는 Node 는 n.right 로 새로 태어나는거야
            # 여기서 del_min 에 넣어주는 인자는 변경 전 n(target)의 right를 
            # 지정해줘야 하겠지? target 의 child 는 아직 유효하거든
            # del_min() 은 기존의 target.right 밑에있는 min 값을 재지정(삭제)
            # 해준 후에 알맞은 n.right 값을 리턴해줄거야
            n.right = self.del_min(target.right)
            # 우리는 min 함수를 이용하기위해 right side 를 실컷 건드렸으니 
            # target 의 left side 는 그대로 사용하면 아무 이상없어 
            # left 를 재지정해주자
            n.left = target.left
        # 최소값 삭제(재지정) 및 트리구조를 제대로 바꾸고 나서 현재 노드를 리턴해.
        # 그러면 parent 의 left or right 의 값이 열심히 변경한 Node 로 바뀌는거야
        return n
```