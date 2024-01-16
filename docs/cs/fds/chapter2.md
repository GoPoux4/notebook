# Lists, Stacks and Queues

## Abstract Data Type (ADT)

**Definition**: \(\text{Data Type} = \{\text{Objects}\} \cup \{\text{Operations}\}\)

将数据定义为抽象的对象集合，不考虑 object 和 operation 内部具体实现细节。

## The List ADT

### ADT

- **Objects**: \((\text{item}_0, \text{item}_1, \cdots, \text{item}_{N-1})\)
- **Operations**: Find length, Print, Make empty list, **Find k-th**, **Insert**, **Delete**, ...

### Array Implementation

\(\text{array}[i] = \text{item}_i\)

- **MaxSize**: has to be estimate 需要预估最大长度
- **Find k-th**: \(O(1)\)
- **Insert** and **Delete**: \(O(N)\)

### Linked List

- **Insert**:

    ```c
    // Insert x after p
    newnode = malloc(sizeof(struct Node));
    newnode->item = x;
    newnode->next = p->next;
    p->next = newnode;
    ```
- **Delete**:

    ```c
    // Delete node p->next
    q = p->next;
    p->next = q->next;
    free(q);
    ```

!!! note "Double Linked List"
    
    ```c
    struct Node {
        Item item;
        struct Node *llink;
        struct Node *rlink;
    };
    ```
### Applications

#### The Polynomial ADT

**Objects**: pair \(\langle \text{exponent}, \text{coefficient} \rangle\)

#### Multilists

存储二维数据。

!!! info "Linked List Matrix"

    每行两个链表存储非零元素和列地址。

#### Cursor Implementation

游标实现，即数组模拟链表。

## The Stack ADT

### ADT

**LIFO**: Last In First Out

### Implimentation

- **Linked List**
- **Array**

### Applications

- 括号匹配
- postfix evaluation 后缀表达式求值
- infix to postfix 中缀表达式转后缀表达式
- Function calls 函数调用，系统栈

## The Queue ADT

### ADT

**FIFO**: First In First Out

### Implementation

**Array Implementation**

!!! info "Circular Queue"

    若不存储队列长度，则无法判断队列为空还是满。

    一般做法为浪费一个空间或者存储队列长度。

## 错题整理

!!! question "Question 1"

    To merge two singly linked ascending lists, both with N nodes, into one singly linked ascending list, the minimum possible number of comparisons is ______.

    !!! note "Answer"

        \(N\)