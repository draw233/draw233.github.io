# Java实现栈

抽取共有代码

```Java
/**
 * @author mirau on 2021/4/15.
 * @version 1.0
 */
public abstract class MyStack<E> {

    int size;

    public int size() {
        return size;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    abstract boolean push(E ele);

    abstract E peek();

    abstract E pop();

}
```

## 基于数组

```Java
public class ArrayStack<E> extends MyStack<E> {

    private final Object[] elementData;

    public ArrayStack() {
        this(10);
    }

    public ArrayStack(int capacity) {
        this.elementData = new Object[capacity];
    }

    @Override
    public boolean isEmpty() {
        return size == 0;
    }

    private boolean isFull() {
        return size == elementData.length;
    }

    @Override
    public boolean push(E ele) {
        if (isFull()) {
            return false;
        }
        elementData[size++] = ele;
        return true;
    }

    @Override
    public E peek() {
        if (isEmpty()) {
            return null;
        }
        E ele = (E) elementData[size - 1];
        return ele;
    }

    @Override
    public E pop() {
        if (isEmpty()) {
            return null;
        }
        E ele = (E) elementData[--size];
        return ele;
    }
}
```

## 基于链表

```Java
public class ListNodeStack<E> extends MyStack<E> {

    private ListNode<E> head;

    @Override
    public boolean push(E ele) {
        ListNode<E> node = new ListNode<>(ele);
        // 往头部加数据
        if (head != null) {
            node.next = head;
        }
        head = node;
        size++;
        return true;
    }

    @Override
    public E peek() {
        // 从头部取数据
        if (head == null) {
            return null;
        } else {
            return head.data;
        }
    }

    @Override
    public E pop() {
        if (head == null) {
            return null;
        } else {
            E data = head.data;
            head = head.next;
            size--;
            return data;
        }
    }

    /**
     * 单链表
     *
     * @param <E> 数据类型
     */
    private static class ListNode<E> {
        private final E data;
        private ListNode<E> next;

        public ListNode(E data) {
            this.data = data;
        }
    }
}
```

## 测试

```Java
public class StackTest {
    public static void main(String[] args) {
//        MyStack<String> stack = new ArrayStack<>();
        MyStack<String> stack = new ListNodeStack<>();
        for (int i = 0; i < 5; i++) {
            if (!stack.push("data" + i)) {
                break;
            }
        }
        System.out.println(stack.size());
        System.out.println(stack.pop());
        System.out.println(stack.peek());
        stack.push("hezhou");
        while (!stack.isEmpty()) {
            System.out.println(stack.pop());
        }
    }
}
```
