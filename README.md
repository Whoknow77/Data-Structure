# 자료구조(Python,JAVA)

## Stack

- 정의   
  LIFO(Last in First Out)구조를 따르고 선입선출인 큐와 반대되는 개념이다.   
  들어오고 나가는 통로가 한 쪽만 있으므로, 한 쪽으로만 자료를 넣고 뺄 수 있다.   

- 연산   
  + push(data): 윗 부분에 데이터 추가
  + pop() : 가장 윗 부분의 데이터 삭제
  + peek() : 가장 윗 부분의 데이터 반환
  + imEmpty() : 스택이 비어 있을 때 True 반환
  + isFull() : 스택이 꽉 찼을 때 True 반환


- 기본 구조
  ```python
  def isStackFull():
    global SIZE,stack,top
    if(top>=SIZE-1):
      return True
    else:
      return False

  def isStackEmpty():
    global SIZE,stack,top
    if(top==-1):
      return True
    else:
      return False

  def push(data):
    global SIZE,stack,top
    if(isStackFull()):
      return
    top+=1
    stack[top]=data

  def pop():
    global SIZE,stack,top
    if(isStackEmpty()):
      return None
    data=stack[top]
    stack[top]=None
    top-=1
    return data

  SIZE=100
  stack=[None for _ in range(SIZE)]
  top=-1
  ```


- 응용문제(괄호의 매칭 검사)   
괄호가 여러 개 사용 된 수식에서 짝이 올바른지 검사하는 코드

```python
import webbrowser
import time

def isStackFull():
  global SIZE,stack,top
  if(top>=SIZE-1):
    return True
  else:
    return False

def isStackEmpty():
  global SIZE,stack,top
  if(top==-1):
    return True
  else:
    return False

def push(data):
  global SIZE,stack,top
  if(isStackFull()):
    return
  top+=1
  stack[top]=data

def pop():
  global SIZE,stack,top
  if(isStackEmpty()):
    return None
  data=stack[top]
  stack[top]=None
  top-=1
  return data

def checkBracket(expr):
  for ch in expr:
    if ch in '({[<': #열린 괄호는 무조건 넣음
      push(ch)
    elif ch in ')]}>': #닫힌괄호는 무조건 삭제
      out=pop()
      if ch==')'and out=='(': #pop된것과 ch가 짝이맞으면 넘어감
        pass
      elif ch=='>'and out=='<':
        pass
      elif ch== '}' and out== '{':
        pass
      elif ch==']' and out=='[':
        pass
      else:
        return False
    else:
      pass
  if isStackEmpty(): #스택이 비어있음
    return True
  else:
    return False



SIZE=100
stack=[None for _ in range(SIZE)]
top=-1

exprAry=['(A+B)', ')A+b(', '((A+b)-C', '(A+B]', '(<A+{B-C}/[C*D]>)']
for expr in exprAry:
  top=-1 # 스택 비우기
  print(expr, '==>', checkBracket(expr))
```

하지만 실제로는 그냥 리스트를 쓰면 된다. 리스트구조랑 스택 구조가 같다.(파이썬에서는 스택 라이브러리 따로 제공 X)


## Queue

- 정의   
  FIFO(First in First Out)구조를 따르고 후입선출인 스택과 반대되는 개념이다.   
  들어오는 입구와 나가는 입구가 다르고, 한쪽으로만 들어오고 한쪽으로만 나가는 개념으로, 은행 대기열, 줄 서는것을 생각하면 된다.


- 연산   
  + enqueue(data): 순서대로 데이터 추가
  + dequeue() : 가장 앞(출구) 부분의 데이터 삭제
  + peek() : 가장 앞(출구) 부분의 데이터 반환
  + imEmpty() : 큐가 비어 있을 때 True 반환
  + isFull() : 큐가 꽉 찼을 때 True 반환   

- 기본 구조
  ```python
  def isFull():
  global SIZE,queue,front,rear
  if(rear!=SIZE-1):
    return False
  elif(rear==SIZE-1) and (front==-1):
    return True
  else:
    for i in range(front+1, SIZE):
      queue[i-1]=queue[i]
      queue[i]=None
    front-=1
    rear-=1
    return False

  def isEmpty():
    global SIZE,queue,front,rear
    if(front==rear):
      return True
    else:
      return False

  def enQueue(data):
    global SIZE,queue,front,rear
    if(isFull()):
      print('큐꽉참')
      return
    rear+=1
    queue[rear]=data

  def deQueue():
    global SIZE,queue,front,rear
    if(isEmpty()):
      print('빔')
      return None
    front+=1
    data=queue[front]
    queue[front]=None
    return data

  def peek(): #제일 먼저 들어간 값 추출
    global SIZE,queue,front,rear
    if(isEmpty()):
      print('빔')
      return
    return queue[front+1]

  SIZE=int(input('큐 크기 입력'))
  queue=[None for _ in range(SIZE)]
  front=rear=-1

  select=input('I,E,V,X중 하나선택')

  while(select!='X' and select!='x'):
    if(select=='I' or select=='i'):
      data=input('입력할 데이터')
      enQueue(data)
      print(queue)
    elif(select=='E' or select=='e'):
      data=deQueue()
      print(queue)
    elif(select=='V' or select=='v'):
      data=peek()
      print(data)
      print(queue)
    else:
      print('입력 잘못됨')

    select=input('I,E,V,X중 하나선택')


    print('프로그램 종료')



- 원형 큐

크기가 매우 큰 큐에서 앞쪽이 비어있을때 enqueue를 하려면 매우 큰 단위의 데이터의 이동이 일어나서 손해를 보게 되는데, 그것을 발전시킨 것이 원형큐이다.   
rear 값을 오른쪽으로 한칸 이동시켜 enqueue하면된다.   
하지만 원형큐에서는 항상 전체에서 한 칸을 적게 사용한다.(비워둔다)   

함수 내에서 순차큐와 다른점은   
+ isFull()함수가 다른 점 
+ 다음 칸을 계산할 때 (현재 위치 + 1)을 큐 크기로 나눈다는 점  
+ front와 rear의 초깃값이 0이라는 점

```python
def isFull():
  global SIZE,queue,front,rear
  if((rear+1)%SIZE==front):
    return True
  else:
    return False


def isEmpty():
  global SIZE,queue,front,rear
  if(front==rear):
    return True
  else:
    return False

def enQueue(data):
  global SIZE,queue,front,rear
  if(isFull()):
    print('큐꽉참')
    return
  rear=(rear+1)%SIZE
  queue[rear]=data

def deQueue():
  global SIZE,queue,front,rear
  if(isEmpty()):
    print('빔')
    return None
  front=(front+1)%SIZE
  data=queue[front]
  queue[front]=None
  return data

def peek(): #제일 먼저 들어간 값 추출
  global SIZE,queue,front,rear
  if(isEmpty()):
    print('빔')
    return
  return queue[(front+1)%SIZE]

SIZE=int(input('큐 크기 입력'))
queue=[None for _ in range(SIZE)]
front=rear=0

select=input('I,E,V,X중 하나선택')

while(select!='X' and select!='x'):
  if(select=='I' or select=='i'):
    data=input('입력할 데이터')
    enQueue(data)
    print(queue)
  elif(select=='E' or select=='e'):
    data=deQueue()
    print(queue)
  elif(select=='V' or select=='v'):
    data=peek()
    print(data)
    print(queue)
  else:
    print('입력 잘못됨')

  select=input('I,E,V,X중 하나선택')


print('프로그램 종료')

```

- 라이브러리 사용법



## deQue(덱)

deque는 스택과 큐를 합친 자료구조이다.  
가장자리에 원소를 넣거나 뺄 수 있고 많이 사용 된다.


- 스택과 큐를 list로 이용하지 않는 이유  

*스택에서 list.append와 list.pop()을 이용했던 것처럼 list.append와 list.pop(0)을 이용하면 리스트를 큐처럼 사용할 수 있다. 하지만 pop()의 time complexity는 O(1)인 반면 pop(0)의 time complexity는 O(N)이기 때문에 시간이 오래 걸린다. 따라서 시간 복잡도를 고려해 리스트는 큐로 사용하지 않는다.*  

- 연산
    + deque(iterable, [maxlen]) : 초기화 함수, 리스트(iterable)을 인자로 건내면 이를 deque화 시켜줌


    + append(x) : x를 오른쪽에 삽입

    + popleft :  가장 왼쪽의 있는 원소를 제거하고, 그 값을 리턴함

    + clear : 모든 원소를 지운다.

- 라이브러리 사용법
    ```python
    from collections import deque
    d=deque()
    d2=duque([1,2,3]) 등으로 설정
    ```

## ArrayList(배열)
- 정의  
  추가/삭제가 느리지만 인덱스로 접근하므로 탐색에 용이하다.

```java
package list.arraylist.implementation;
 
public class ArrayList {
    private int size = 0;
    private Object[] elementData = new Object[100];
 
    public ArrayList() {
 
    }
     
    public boolean addLast(Object element) {
        elementData[size] = element;
        size++;
        return true;
    }
     
    public boolean add(int index, Object element) {
        // 엘리먼트 중간에 데이터를 추가하기 위해서는 끝의 엘리먼트부터 index의 노드까지 뒤로 한칸씩 이동시켜야 합니다.
        for (int i = size - 1; i >= index; i--) {
            elementData[i + 1] = elementData[i];
        }
        // index에 노드를 추가합니다.
        elementData[index] = element;
        // 엘리먼트의 숫자를 1 증가 시킵니다.
        size++;
        return true;
    }
     
    public boolean addFirst(Object element){
        return add(0, element);
    }
 
    public String toString() {
        String str = "[";
        for (int i = 0; i < size; i++) {
            str += elementData[i];
            if (i < size - 1)
                str += ",";
        }
        return str + "]";
    }
     
    public Object remove(int index) {
        // 엘리먼트를 삭제하기 전에 삭제할 데이터를 removed 변수에 저장합니다.
        Object removed = elementData[index];
        // 삭제된 엘리먼트 다음 엘리먼트부터 마지막 엘리먼트까지 순차적으로 이동해서 빈자리를 채웁니다.
        for (int i = index + 1; i <= size - 1; i++) {
            elementData[i - 1] = elementData[i];
        }
        // 크기를 줄입니다.
        size--;
        // 마지막 위치의 엘리먼트를 명시적으로 삭제해줍니다. 
        elementData[size] = null;
        return removed;
    }   
     
    public Object removeFirst(){
        return remove(0);
    }
 
    public Object removeLast(){
        return remove(size-1);
    }
 
    public Object get(int index) {
        return elementData[index];
    }
 
    public int size() {
        return size;
    }
 
    public int indexOf(Object o) {
        for (int i = 0; i < size; i++) {
            if (o.equals(elementData[i])) {
                return i;
            }
        }
        return -1;
    }
 
    public ListIterator listIterator() {
        // ListIterator 인스턴스를 생성해서 리턴합니다.
        return new ListIterator();
    }
 
     
 
    class ListIterator {
        // 현재 탐색하고 있는 순서를 가르키는 인덱스 값
        private int nextIndex = 0;
 
        // next 메소르를 호출할 수 있는지를 체크합니다.
        public boolean hasNext() {
            // nextIndex가 엘리먼트의 숫자보다 적다면 next를 이용해서 탐색할 엘리먼트가 존재하는 것이기 때문에 true를 리턴합니다. 
            return nextIndex < size();
        }
         
        // 순차적으로 엘리먼트를 탐색해서 리턴합니다. 
        public Object next() {
            // nextIndex에 해당하는 엘리먼트를 리턴하고 nextIndex의 값을 1 증가 시킵니다.
            return elementData[nextIndex++];
        }
         
        // previous 메소드를 호출해도 되는지를 체크합니다.
        public boolean hasPrevious(){
            // nextIndex가 0보다 크다면 이전 엘리먼트가 존재한다는 의미입니다.
            return nextIndex > 0;
        }
         
        // 순차적으로 이전 노드를 리턴합니다.
        public Object previous(){
            // 이전 엘리먼트를 리턴하고 nextIndex의 값을 1감소합니다. 
            return elementData[--nextIndex];
        }
         
        // 현재 엘리먼트를 추가합니다. 
        public void add(Object element){
            ArrayList.this.add(nextIndex++, element);
        }
         
        // 현재 엘리먼트를 삭제합니다. 
        public void remove(){
            ArrayList.this.remove(nextIndex-1);
            nextIndex--;
        }
         
 
    }
 
}
```




## LinkedList(단일 연결 리스트)
- 정의  
  각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식의 자료구조, 노드의 포인터가 다음 노드와의 연결을 담당한다.

  배열에 비해서 추가/삭제가 용이하나, 인덱스가 없는 리스트의 특징으로 인하여 접근 시에는 속도가 떨어진다.

```java

package list.linkedlist.implementation;
 
public class LinkedList {
    // 첫번째 노드를 가리키는 필드
    private Node head;
    private Node tail;
    private int size = 0;
    private class Node{
        // 데이터가 저장될 필드
        private Object data;
        // 다음 노드를 가리키는 필드
        private Node next;
        public Node(Object input) {
            this.data = input;
            this.next = null;
        }
        // 노드의 내용을 쉽게 출력해서 확인해볼 수 있는 기능
        public String toString(){
            return String.valueOf(this.data);
        }
    }
    public void addFirst(Object input){
        // 노드를 생성합니다.
        Node newNode = new Node(input);
        // 새로운 노드의 다음 노드로 해드를 지정합니다.
        newNode.next = head;
        // 헤드로 새로운 노드를 지정합니다.
        head = newNode;
        size++;
        if(head.next == null){
            tail = head;
        }
    }
    public void addLast(Object input){
        // 노드를 생성합니다.
        Node newNode = new Node(input);
        // 리스트의 노드가 없다면 첫번째 노드를 추가하는 메소드를 사용합니다.
        if(size == 0){
            addFirst(input);
        } else {
            // 마지막 노드의 다음 노드로 생성한 노드를 지정합니다.
            tail.next = newNode;
            // 마지막 노드를 갱신합니다.
            tail = newNode;
            // 엘리먼트의 개수를 1 증가 시킵니다.
            size++;
        }
    }
    Node node(int index) {
        Node x = head;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    }
    public void add(int k, Object input){
        // 만약 k가 0이라면 첫번째 노드에 추가하는 것이기 때문에 addFirst를 사용합니다.
        if(k == 0){
            addFirst(input);
        } else {
            Node temp1 = node(k-1);
            // k 번째 노드를 temp2로 지정합니다.
            Node temp2 = temp1.next;
            // 새로운 노드를 생성합니다.
            Node newNode = new Node(input);
            // temp1의 다음 노드로 새로운 노드를 지정합니다.
            temp1.next = newNode;
            // 새로운 노드의 다음 노드로 temp2를 지정합니다.
            newNode.next = temp2;
            size++;
            // 새로운 노드의 다음 노드가 없다면 새로운 노드가 마지막 노드이기 때문에 tail로 지정합니다.
            if(newNode.next == null){
                tail = newNode;
            }
        }
    }
    public String toString() {
        // 노드가 없다면 []를 리턴합니다.
        if(head == null){
            return "[]";
        }       
        // 탐색을 시작합니다.
        Node temp = head;
        String str = "[";
        // 다음 노드가 없을 때까지 반복문을 실행합니다.
        // 마지막 노드는 다음 노드가 없기 때문에 아래의 구문은 마지막 노드는 제외됩니다.
        while(temp.next != null){
            str += temp.data + ",";
            temp = temp.next;
        }
        // 마지막 노드를 출력결과에 포함시킵니다.
        str += temp.data;
        return str+"]";
    }
    public Object removeFirst(){
        // 첫번째 노드를 temp로 지정하고 head의 값을 두번째 노드로 변경합니다.
        Node temp = head;
        head = temp.next;
        // 데이터를 삭제하기 전에 리턴할 값을 임시 변수에 담습니다. 
        Object returnData = temp.data;
        temp = null;
        size--;
        return returnData;
    }
    public Object remove(int k){
        if(k == 0)
            return removeFirst();
        // k-1번째 노드를 temp의 값으로 지정합니다.
        Node temp = node(k-1);
        // 삭제 노드를 todoDeleted에 기록해 둡니다. 
        // 삭제 노드를 지금 제거하면 삭제 앞 노드와 삭제 뒤 노드를 연결할 수 없습니다.  
        Node todoDeleted = temp.next;
        // 삭제 앞 노드의 다음 노드로 삭제 뒤 노드를 지정합니다.
        temp.next = temp.next.next;
        // 삭제된 데이터를 리턴하기 위해서 returnData에 데이터를 저장합니다.
        Object returnData = todoDeleted.data; 
        if(todoDeleted == tail){
            tail = temp;
        }
        // cur.next를 삭제 합니다.
        todoDeleted = null; 
        size--;
        return returnData;
    }
    public Object removeLast(){
        return remove(size-1);
    }
    public int size(){
        return size;
    }
    public Object get(int k){
        Node temp = node(k);
        return temp.data;
    }
    public int indexOf(Object data){
        // 탐색 대상이 되는 노드를 temp로 지정합니다.
        Node temp = head;
        // 탐색 대상이 몇번째 엘리먼트에 있는지를 의미하는 변수로 index를 사용합니다.
        int index = 0;
        // 탐색 값과 탐색 대상의 값을 비교합니다. 
        while(temp.data != data){
            temp = temp.next;
            index++;
            // temp의 값이 null이라는 것은 더 이상 탐색 대상이 없다는 것을 의미합니다.이 때 -1을 리턴합니다.
            if(temp == null)
                return -1;
        }
        // 탐색 대상을 찾았다면 대상의 인덱스 값을 리턴합니다.
        return index;
    }
 
    // 반복자를 생성해서 리턴해줍니다.
    public ListIterator listIterator() {
        return new ListIterator();
    }
     
    class ListIterator{
        private Node lastReturned;
        private Node next;
        private int nextIndex;
         
        ListIterator(){
            next = head;
            nextIndex = 0;
        }
         
        // 본 메소드를 호출하면 next의 참조값이 기존 next.next로 변경됩니다. 
        public Object next() {
            lastReturned = next;
            next = next.next;
            nextIndex++;
            return lastReturned.data;
        }
         
        public boolean hasNext() {
            return nextIndex < size();
        }
         
        public void add(Object input){
            Node newNode = new Node(input);
            if(lastReturned == null){
                head= newNode;
                newNode.next = next;
            } else {
                lastReturned.next = newNode;
                newNode.next = next;
            }
            lastReturned = newNode;
            nextIndex++;
            size++;
        }
         
        public void remove(){
            if(nextIndex == 0){
                throw new IllegalStateException();
            }
            LinkedList.this.remove(nextIndex-1);
            nextIndex--;
        }
         
    }
 
}
```


## DoublyLinkedList(이중 연결 리스트)
  - 정의  
    노드의 포인터가 이전 노드와의 연결을 담당하는 것을 추가함.  
    따라서 더 간편하지만, 데이터양이 늘어남.  
    
    자바에서의 LinkedList 프레임워크는 DoublyLinkedList를 의미한다.


```java
package list.doublylinkedlist.implementation;
 
public class DoublyLinkedList {
    // 첫번째 노드를 가리키는 필드
    private Node head;
    private Node tail;
    private int size = 0;
 
    private class Node {
        // 데이터가 저장될 필드
        private Object data;
        // 다음 노드를 가리키는 필드
        private Node next;
        private Node prev;
 
        public Node(Object input) {
            this.data = input;
            this.next = null;
            this.prev = null;
        }
 
        // 노드의 내용을 쉽게 출력해서 확인해볼 수 있는 기능
        public String toString() {
            return String.valueOf(this.data);
        }
    }
 
    public void addFirst(Object input) {
        // 노드를 생성합니다.
        Node newNode = new Node(input);
        // 새로운 노드의 다음 노드로 헤드를 지정합니다.
        newNode.next = head;
        // 기존에 노드가 있었다면 현재 헤드의 이전 노드로 새로운 노드를 지정합니다.
        if (head != null)
            head.prev = newNode;
        // 헤드로 새로운 노드를 지정합니다.
        head = newNode;
        size++;
        if (head.next == null) {
            tail = head;
        }
    }
 
    public void addLast(Object input) {
        // 노드를 생성합니다.
        Node newNode = new Node(input);
        // 리스트의 노드가 없다면 첫번째 노드를 추가하는 메소드를 사용합니다.
        if (size == 0) {
            addFirst(input);
        } else {
            // tail의 다음 노드로 생성한 노드를 지정합니다.
            tail.next = newNode;
            // 새로운 노드의 이전 노드로 tail을 지정합니다.
            newNode.prev = tail;
            // 마지막 노드를 갱신합니다.
            tail = newNode;
            // 엘리먼트의 개수를 1 증가 시킵니다.
            size++;
        }
    }
 
    Node node(int index) {
        // 노드의 인덱스가 전체 노드 수의 반보다 큰지 작은지 계산
        if (index < size / 2) {
            // head부터 next를 이용해서 인덱스에 해당하는 노드를 찾습니다.
            Node x = head;
            for (int i = 0; i < index; i++)
                x = x.next;
            return x;
        } else {
            // tail부터 prev를 이용해서 인덱스에 해당하는 노드를 찾습니다.
            Node x = tail;
            for (int i = size - 1; i > index; i--)
                x = x.prev;
            return x;
        }
    }
 
    public void add(int k, Object input) {
        // 만약 k가 0이라면 첫번째 노드에 추가하는 것이기 때문에 addFirst를 사용합니다.
        if (k == 0) {
            addFirst(input);
        } else {
            Node temp1 = node(k - 1);
            // k 번째 노드를 temp2로 지정합니다.
            Node temp2 = temp1.next;
            // 새로운 노드를 생성합니다.
            Node newNode = new Node(input);
            // temp1의 다음 노드로 새로운 노드를 지정합니다.
            temp1.next = newNode;
            // 새로운 노드의 다음 노드로 temp2를 지정합니다.
            newNode.next = temp2;
            // temp2의 이전 노드로 새로운 노드를 지정합니다.
            if (temp2 != null)
                temp2.prev = newNode;
            // 새로운 노드의 이전 노드로 temp1을 지정합니다.
            newNode.prev = temp1;
            size++;
            // 새로운 노드의 다음 노드가 없다면 새로운 노드가 마지막 노드이기 때문에 tail로 지정합니다.
            if (newNode.next == null) {
                tail = newNode;
            }
        }
    }
 
    public String toString() {
        // 노드가 없다면 []를 리턴합니다.
        if (head == null) {
            return "[]";
        }
        // 탐색을 시작합니다.
        Node temp = head;
        String str = "[";
        // 다음 노드가 없을 때까지 반복문을 실행합니다.
        // 마지막 노드는 다음 노드가 없기 때문에 아래의 구문은 마지막 노드는 제외됩니다.
        while (temp.next != null) {
            str += temp.data + ",";
            temp = temp.next;
        }
        // 마지막 노드를 출력결과에 포함시킵니다.
        str += temp.data;
        return str + "]";
    }
 
    public Object removeFirst() {
        // 첫번째 노드를 temp로 지정하고 head의 값을 두번째 노드로 변경합니다.
        Node temp = head;
        head = temp.next;
        // 데이터를 삭제하기 전에 리턴할 값을 임시 변수에 담습니다.
        Object returnData = temp.data;
        temp = null;
        // 리스트 내에 노드가 있다면 head의 이전 노드를 null로 지정합니다.
        if (head != null)
            head.prev = null;
        size--;
        return returnData;
    }
 
    public Object remove(int k) {
        if (k == 0)
            return removeFirst();
        // k-1번째 노드를 temp로 지정합니다.
        Node temp = node(k - 1);
        // temp.next를 삭제하기 전에 todoDeleted 변수에 보관합니다.
        Node todoDeleted = temp.next;
        // 삭제 대상 노드를 연결에서 분리합니다.
        temp.next = temp.next.next;
        if (temp.next != null) {
            // 삭제할 노드의 전후 노드를 연결합니다.
            temp.next.prev = temp;
        }
        // 삭제된 노드의 데이터를 리턴하기 위해서 returnData에 데이터를 저장합니다.
        Object returnData = todoDeleted.data;
        // 삭제된 노드가 tail이었다면 tail을 이전 노드를 tail로 지정합니다.
        if (todoDeleted == tail) {
            tail = temp;
        }
        // cur.next를 삭제 합니다.
        todoDeleted = null;
        size--;
        return returnData;
    }
 
    public Object removeLast() {
        return remove(size - 1);
    }
 
    public int size() {
        return size;
    }
 
    public Object get(int k) {
        Node temp = node(k);
        return temp.data;
    }
 
    public int indexOf(Object data) {
        // 탐색 대상이 되는 노드를 temp로 지정합니다.
        Node temp = head;
        // 탐색 대상이 몇번째 엘리먼트에 있는지를 의미하는 변수로 index를 사용합니다.
        int index = 0;
        // 탐색 값과 탐색 대상의 값을 비교합니다.
        while (temp.data != data) {
            temp = temp.next;
            index++;
            // temp의 값이 null이라는 것은 더 이상 탐색 대상이 없다는 것을 의미합니다.이 때 -1을 리턴합니다.
            if (temp == null)
                return -1;
        }
        // 탐색 대상을 찾았다면 대상의 인덱스 값을 리턴합니다.
        return index;
    }
 
    // 반복자를 생성해서 리턴해줍니다.
    public ListIterator listIterator() {
        return new ListIterator();
    }
 
    public class ListIterator {
        private Node lastReturned;
        private Node next;
        private int nextIndex;
 
        ListIterator() {
            next = head;
            nextIndex = 0;
        }
 
        // 본 메소드를 호출하면 cursor의 참조값이 기존 cursor.next로 변경됩니다.
        public Object next() {
            lastReturned = next;
            next = next.next;
            nextIndex++;
            return lastReturned.data;
        }
 
        // cursor의 값이 없다면 다시 말해서 더 이상 next를 통해서 가져올 노드가 없다면 false를 리턴합니다.
        // 이를 통해서 next를 호출해도 되는지를 사전에 판단할 수 있습니다.
        public boolean hasNext() {
            return nextIndex < size();
        }
 
        public boolean hasPrevious() {
            return nextIndex > 0;
        }
 
        public Object previous() {
            if (next == null) {
                lastReturned = next = tail;
            } else {
                lastReturned = next = next.prev;
            }
            nextIndex--;
            return lastReturned.data;
        }
 
        public void add(Object input) {
            Node newNode = new Node(input);
            if (lastReturned == null) {
                head = newNode;
                newNode.next = next;
            } else {
                lastReturned.next = newNode;
                newNode.prev = lastReturned;
                if (next != null) {
                    newNode.next = next;
                    next.prev = newNode;
                } else {
                    tail = newNode;
                }
            }
            lastReturned = newNode;
            nextIndex++;
            size++;
        }
 
        public void remove() {
            if (nextIndex == 0) {
                throw new IllegalStateException();
            }
            Node n = lastReturned.next;
            Node p = lastReturned.prev;
 
            if (p == null) {
                head = n;
                head.prev = null;
                lastReturned = null;
            } else {
                p.next = next;
                lastReturned.prev = null;
            }
 
            if (n == null) {
                tail = p;
                tail.next = null;
            } else {
                n.prev = p;
            }
 
            if (next == null) {
                lastReturned = tail;
            } else {
                lastReturned = next.prev;
            }
 
            size--;
            nextIndex--;
 
        }
    }
}
```

## Tree

-  정의

    데이터가 순차적으로 나열되어진 형태인 스택과 큐와 다르게 데이터가 계층적(망)으로 구성되어있는 비선형구조이다. 선형구조는 저장과 꺼내는 것에 초점이 맞춰져 있다면 비선형구조는 **표현**에 초점이 맞춰져 있다.

- 개념
  
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmzvPd%2FbtqRGpOYUdG%2Fw9mXPrxMctUj36UFxKtpu1%2Fimg.png">

  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F1ewj6%2FbtqRQR4CRlS%2Fc1jkNNODJRuh4WglfaNpT1%2Fimg.png">

  - 조상 : 노드의 부모 노드들의 총 집합
   
  - 자손 : 노드의 서브트리에 있는 모든 노드
   
  - 레벨 : 루트 노드들로부터의 깊이(루트 노드의 레벨 = 1)
  
  - 트리의 깊이 : 트리에 속한 노드의 최대 레벨
 
  - 노드의 차수 : 노드가 가지고 있는 자식 노드의 개수
  
  - 트리의 차수 : 트리가 가지고 있는 노드의 차수 중 가장 큰 값

- 이진 트리

  - 정의

    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fn1PZV%2FbtqROfdGty9%2F5USJxhRdFSqHVTfl8CIgV0%2Fimg.png">

    공집합이거나 루트와 왼쪽 서브트리, 오른쪽 서브트리로 구성된 노드들의 유한 집합

  - 성질

    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmprJF%2FbtqROe6RYWw%2FKlE9Obcn0QLpcxfIFExYDk%2Fimg.png">

    - n개의 노드를 가진 이진트리는 n-1개의 간선을 가진다.

    - 부모와 자신간에는 정확하게 하나의 간선만이 존재한다.
    
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKaUbx%2FbtqRI7UKjZG%2FhzgMOHxMyLM5amHCFJeGdK%2Fimg.png">

    - 높이가 h인 이진트리의 경우, 최소 h개의 노드를 가지고 최대 2^h - 1개의 노드를 가진다.
    

    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FxXByP%2FbtqRQS3zrlN%2FJwFQkVjTN6grc9cutDCK1k%2Fimg.png">

    - n개의 노드를 가지는 이진트리의 높이는 최대 n 이거나 최소 log(n+1)이 된다.

  - 분류
  

    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBJhFF%2FbtqRAjuXfQD%2F5TQLPypf3At1AMPUUUqGgK%2Fimg.png">

    - 포화 이진트리 : 각 레벨에 노드가 꽉 차있는 노드

    - 완전 이진트리 : 높이가 k일때 레벨1부터 레벨 k-1까지는 노드가 모두 채워져 있고 레벨K부터는 왼쪽부터 오른쪽으로 순서대로 노드가 채워져있다. 마지막 레벨에 노드가 꽉 차 있지 않아도 되지만 중간에 빈 곳이 있어서는 안된다.

    - 포화 이진트리는 완전 이진트리에 속하지만 그 역은 성립되지 않는다.


  - 순회

    트리 구조는 비 선형 자료구조이기 때문에 for문 한번으로 방문이 불가

    모든 순회는 루트 노드에서 시작


    - 전위 순회(preOrder)

      1) 현재 노드 방문
      
      2) 왼쪽 자식 노드 탐색

      3) 오른쪽 자식 노드 탐색
      

    - 중위 순회(inOrder)

      1) 왼쪽 자식 노드 탐색
      
      2) 현재 노드 방문

      3) 오른쪽 자식 노드 탐색

    - 후위 순회(postOrder)

      1) 왼쪽 자식 노드 탐색
      
      2) 오른쪽 자식 노드 탐색

      3) 현재 노드 방문

    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FdCim19%2FbtqE8x5Zy2t%2F3mvmGuF1aAS4oNTzkSgnI1%2Fimg.png">

  - 예제

```
    package tree;

public class BinaryTree {

	public static void main(String[] args) {
		
		int count=7;
		
		Node[] nodeList = new Node[count+1];
		
		for(int i=1; i<=count; i++) {
			Node binaryTree = new Node(i);
			nodeList[i]=binaryTree;
		}
		
		for(int i=1; i<=count; i++) {
			if(i*2<=count) {
				nodeList[i].leftChild=nodeList[i*2];
				nodeList[i].rightChild=nodeList[(i*2)+1];
			}
		}
		System.out.println("전위 순회");
		preOrder(nodeList[1]);
		System.out.println();
		
		System.out.println("중위 순회");
		inOrder(nodeList[1]);
		System.out.println();
		
		System.out.println("후위 순회");
		postOrder(nodeList[1]);
		System.out.println();
	}
	
	static void preOrder(Node node) { //전위 순회
		
		if(node!=null) {
			System.out.print(node.data+" "); // 현재 노드 방문
			preOrder(node.leftChild); // 왼쪽 자식 노드 탐색
			preOrder(node.rightChild); // 오른쪽 자식 노드 탐색
		}
	}
	
	static void inOrder(Node node) { // 중위 순회
		
		if(node!=null) {
			inOrder(node.leftChild); // 왼쪽 자식 노드 탐색
			System.out.print(node.data + " "); //현재 노드 방문
			inOrder(node.rightChild); // 오른쪽 자식 노드 탐색
		}
	}
	
	static void postOrder(Node node) { //후위 순회
		
		if(node!=null) {
			postOrder(node.leftChild); //왼쪽 자식 노드 탐색
			postOrder(node.rightChild); //오른쪽 자식 노드 탐색
			System.out.print(node.data + " "); //현재 노드 방문
		}
	}
    }
```


## 힙(Heap)

  - 정의
    
    이진 트리의 응용 버전

    완전 이진트리 형태의 자료구조

    일차원 배열로 구현

    일반적으로 그룹을 정렬하거나 입력된 데이터 안에서 최소/최대 값을 찾을 때 사용함.

    데이터의 삽입과 삭제가 빠르며, 각각의 수행시간 : O(log N)

  - 형태

    - 최대 힙
      
      각 노드의 키 값은 자식 노드의 키 값 보다 크다.

    - 최소 힙

      각 노드의 키 값은 자식 노드의 키 값 보다 작다.

  - 삽입 연산

    1) 트리의 가장 마지막 위치에 노드를 삽입

    2) 추가된 노드와 그 부모 노드가 힙 조건을 만족하는지 확인

    3) 만족 하지 않음녀 부모와 자식의 키 Swap

    4) 조건에 만족하거나 추가된 노드가 루트에 도달할 때까지 2~3번 반복

      완전 이진 트리의 리프 노드부터 루트 노드까지 연산

      노드가 N개일 때 수행 시간 O(log N) : 아래에서 위로 갈수록 수행 횟수 반으로 줄어듬.

  - 삭제 연산

    1) 힙의 삭제연산은 항상 루트 노드를 삭제

    2) 트리의 가장 마지막 노드를 루트 자리로 삽입

    3) 바꾼 위치의 노드가 힙 조건을 만족하는지 확인

    4) 만족하지 않는다면 왼쪽 자식과 오른쪽 자식 중 적합한 노드와 키 값을 바꿈

    5) 조건을 만족하거나 리프 노드에 도달할 때까지 3~4번을 반복

    
    완전 이진 트리의 루트 노드부터 리프 노드까지 연산을 함.

    노드가 N개일 때 수행 시간 O(log N)

  - 최대힙 예제

    ```
    package Heap;

    import java.util.ArrayList;
    import java.util.Scanner;

    public class MaxHeap {
    
    public static class MaximumHeap{
      
      private ArrayList<Integer> heap; // heap 이름의 int 데이터를 담는 ArrayList 배열 선언
      // 배열과 달리 생성후에 크기가 변한다.
      
      public MaximumHeap() { // 생성자
        heap = new ArrayList<>();
        heap.add(1000000); // 0 인덱스에 안쓸 값 저장
      }
      
      public void print() { // heap 출력
        for(int i=1; i<heap.size(); i++) {
          System.out.print(heap.get(i)+" ");
        }
        System.out.println();
      }
      
      public void insert(int val) { // 가장 마지막에 삽입
        heap.add(val); // 마지막 위치에 노드 삽입
        int p = heap.size() - 1; // 인덱스 값
        
        while(p>1 && heap.get(p/2) < heap.get(p)) { // 루트노드가 아니고, 자식 노드가 더 크다면
          System.out.println("swap"); //swap
          int temp = heap.get(p/2);
          heap.set(p/2, heap.get(p));
          heap.set(p, temp);
          
          
          p=p/2; //부모 노드로 위치 이동
        }
      }
      
      public int delete() { // 항상 루트노드 삭제
        if(heap.size() - 1 < 1) { // 힙 안의 노드가 하나도 없을 때는 삭제 불가
          return 0;
        }
        int deletedItem = heap.get(1); // 삭제할 데이터 저장(루트노드)
        
        heap.set(1, heap.get(heap.size() -1 )); // set() : 임의의 원소 수정 => 루트노드의 값에 마지막 노드 값 대입
        heap.remove(heap.size()-1); // 마지막 노드 삭제
        
        int pos = 1; // 위치(초기에는 루트노드)
        
        while((pos*2) < heap.size()) { // 자식노드가 존재하지 여부
                  
          int maxPos = pos * 2; // 최댓값 위치는 좌측 자식노드 인덱스 값
          
          if(((maxPos+1) < heap.size()) && heap.get(maxPos) < heap.get(maxPos+1)) { // 힙 사이즈가 우측 자식노드위치값보다 큰경우(맨 하단까지) && 최댓값이 우측 자식 노드의 값보다 작은경우 => swap해야하는경우
            maxPos++;
          }
          
          if(heap.get(pos) > heap.get(maxPos)) { // 루트노드가 최댓값일때 아무일도 안일어나게 함
            break;
          }
          
          int temp = heap.get(pos); // swap // 루트노드 값 저장
          heap.set(pos, heap.get(maxPos)); //루트 노드에 최댓값 대입
          heap.set(maxPos, temp); // 최댓값이 있는 노드에 루트노드 대입
          pos = maxPos; // 최댓값이 있는 노드의 위치를 갱신
        }
        
        return deletedItem; // 삭제할 루트노드 값 반환
        
        
      }
      
    }

    
    public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      
      int N = sc.nextInt();
      
      MaximumHeap maximumHeap = new MaximumHeap(); // 최대 힙 객체 생성
      
      for(int i=0; i<N; i++) {
        int val = sc.nextInt();
        
        if(val==0) { // 삭제
          System.out.println(maximumHeap.delete());
        }
        else if(val==-1) { // 힙 출력
          maximumHeap.print();
        }
        
        else { // 삽입
          maximumHeap.insert(val);
        }
      }
    }
      }
		


   





    

  



  









  


















- 출처 

    https://yjshin.tistory.com/entry/
    https://baeharam.netlify.app/posts/data%20structure/hash-table
    https://opentutorials.org/module/1335/8941#
    https://go-coding.tistory.com/7