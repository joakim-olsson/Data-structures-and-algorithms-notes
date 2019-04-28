### Hash Tables

Easy way to implement:

1. First, compute the key's hash code, which will usually be an int or long. Note that two different keys
could have the same hash code, as there may be an infinite number of keys and a finite number of ints.

2. Then, map the hash code to an index in the array. This could be done with something like hash (key)
% array_length. Two different hash codes could, of course, map to the same index.

3. At this index, there is a linked list of keys and values. Store the key and value in this index. We must use a linked list because of collisions: you could have two different keys with the same hash code, or two different hash codes that map to the same index.

To retrieve the value pair by its key, you repeat this process. Compute the hash code from the key, and then compute the index from the hash code. Then, search through the linked list for the value with this key.

If the number of collisions is very high, the worst case runtime is O(N), where N is the number of keys. However, we generally assume a good implementation that keeps collisions to a minimum, in which case the lookup time is a O(1).

To sum up: An array containing linked lists. Add the element to the linkedlist at the specific index, if collision, the linkedlist is now length 2 and so on.

#### Alternative

Use balanced binary search tree, which is an O(log N) lookup time, advantage is that it uses less space.


### ArrayList & Resizable arrays

ArrayList are actually arrays, but are resizable. The internal array will grow as items are appended. Typical implementation is doubling the internal array when full. Each doubling takes O(n) time, but happens so rarely that its amortized insertion i still O(1).

This is because the copying is like this: n/2 + n/4 + n/8 + ... + 2 + 1. This will never pass n, but will get very close.

Therefore, inserting N elements takes O(N) work total. Each insertion is 0(1) on average, even though some insertions take 0 (N) time in the worst case.

### StringBuilder

Concatenating a list of strings with a for loop a new copy of the string is created. If we assume that there are n strings with the same length x, the strings have to be copied over on each concatenation. Making the total time O(x + 2x + ... + nx). This is reduced to O(xn^2) because 1 + 2 + ... + n = n(n+1)/2 or O(n^2). Last + first times the amount of total times divided by 2.

StringBuilder avoids this problem with a resizable array of all strings and only copying them back to a string only when neccesary.

```java
String joinWords(String[] words) {
    StringBuilder sentence = new StringBuilder();
    for (String w : words) {
        sentence.append(w);
    }
    return sentence.toString();
}
```

#### Important Note

For simple string concatenation it is not needed to use StringBuilder, as the compiler will use this automatically. But inside loops it will be needed.

### Linked lists

Have a node class like:

```java
public class LinkedList<T> {
    private ListElement<T> first;   // First element in list.
    private ListElement<T> last;    // Last element in list.
    private int size;               // Number of elements in list.

    /**
     * A list element.
     */
    private static class ListElement<T> {
        public T data;
        public ListElement<T> next;

        public ListElement(T data) {
            this.data = data;
            this.next = null;
        }
    }

    /**
     * Creates an empty list.
     */
    public LinkedList() {
        first = null;
        last = null;
        size = 0;
    }
```

Then adding a node to the last or first would just require a switch from first to first.next and then first to that node.

The runner technique is used a lot with linked list. Uses two pointers and then switches the position of the nodes that they point to.


### Stack

Uses LIFO (last in first out) ordering. Includes methods like pop(), push(item), peek() and isEmpty().

Constant time adds and removes.

```java

public class StackAsLinkedList {

    StackNode root;

    static class StackNode {
        int data;
        StackNode next;

        StackNode(int data) {
            this.data = data;
        }
    }

    public boolean isEmpty() {
        if (root == null)
            return true;
        else
            return false;
    }

    public void push(int data) {
        StackNode newNode = new StackNode(data);

        if (root == null) {
            root = newNode;
        } else {
            StackNode temp = root;
            root = newNode;
            newNode.next = temp;
        }
        System.out.println(data + " pushed to stack");
    }

    public int pop() {
        int popped = Integer.MIN_VALUE;
        if (root == null) {
            System.out.println("Stack is Empty");
        } else {
            popped = root.data;
            root = root.next;
        }
        return popped;
    }

    public int peek() {
        if (root == null) {
            System.out.println("Stack is empty");
            return Integer.MIN_VALUE;
        } else {
            return root.data;
        }           
    }  
}
```

Can be used to implement a recursive algorithm iteratively.

### Queue

Uses FIFO (first-in first-out) ordering. As in a line at at ticket stand. Includes methods like add(item) to the end of the list. remove() the first item. peek() and isEmpty().

Can be implemented with a linked list, actually are essentially the same thing. Besides items are added and removed from opposite sides.

```java

public class MyQueue<T> {
    private staticclass QueueNode<T> {
        private T data;
        private QueueNode<T> next;

        public QueueNode(T data) {
            this.data = data;
        }
    }

    private QueueNode<T> first;
    private QueueNode<T> last;

    public void add(T item) {
        QueueNode<T> t = new QueueNode(item);
        if (last != null)
            last.next = t;
        last = t;
        if (first == null)
            first = last;
    }

    public T remove() {
        if (first == null)
            throw new NoSuchElementException();
        T data = first.data;
        first = first.next;
        if (first == null)
            last = null;
        return data;
    }
    .
    .
    .
}
```

Queue is often used in breadth-first search or in implementing a cahce. breadth-first as we add its adjacent nodes to the back of the queue, allowing to process nodes in the order in which they are viewed.


### Trees and graphs
