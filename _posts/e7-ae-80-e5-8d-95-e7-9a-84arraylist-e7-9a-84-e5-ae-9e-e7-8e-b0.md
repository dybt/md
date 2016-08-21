---
title: 简单的ArrayList的实现
id: 146
categories:  Java复习
date: 2015-07-08 05:11:07
tags: Java
---

[code lang="java"]
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collection;

/**
 * Created by lei on 15-7-8.
 */
public class MyArrayList&lt;E&gt; {
    private Object[] element;

    @Override
    public String toString() {
        StringBuilder toString= new StringBuilder(&quot;&quot;);
        for (int i = 0; i &lt; size; i++) {
            toString.append(i + &quot;: &quot; + element[i] + &quot; &quot;);
        }
        return toString.toString();
    }

    private static final int defaultCapacity = 10;
    private int size = 0;
    private int capacity;

    public MyArrayList() {
        element = new Object[defaultCapacity];
        this.capacity = defaultCapacity;
    }

    public MyArrayList(int capacity) {
        if (capacity &lt; 0) {
            throw new IllegalArgumentException(&quot;hello&quot;);
        }
        element = new Object[capacity];
        this.capacity = capacity;

    }

    public MyArrayList(Collection&lt;? extends E&gt; c) {
        element = c.toArray();
        size = element.length;
        System.out.println(element.getClass());
        System.out.println(Object[].class);
        if (element.getClass() != Object[].class) {
            Arrays.copyOf(element, element.length, Object[].class);
        }
    }

    public void add(E e) {
        ensureCapacity(size);
        element[size++] = e;
    }

    public void add(int index, E e) {
        rangeCheck(index);
        ensureCapacity(size + 1);
        int num = size - index;
        System.arraycopy(element, index, element, index + 1, num);
        element[index] = e;
        size++;
    }

    public boolean addAll(int index, Collection&lt;? extends E&gt; e) {
        rangeCheck(index);
        Object[] temp = e.toArray();

        int num = size - index + temp.length;
        ensureCapacity(size + temp.length);
        System.arraycopy(element, index , element, index + temp.length , size - index );

        System.arraycopy(temp, 0, element, index, temp.length);
        this.size += temp.length;
        return temp.length != 0;
    }

    public boolean addAll(Collection&lt;? extends E&gt; e) {
        Object[]  temp = e.toArray();

        ensureCapacity(size + temp.length);
        System.arraycopy(temp, 0, element, size, temp.length);
        this.size += temp.length;
        return temp.length != 0;
    }

    @SuppressWarnings(&quot;unchecked&quot;)
    public E element(int index) {
        rangeCheck(index);
        Object oldData = element[index];
        return (E) oldData;
    }

    public E remove(int index) {
        rangeCheck(index);

        E oldData = element(index);
        System.arraycopy(element, index + 1, element, index, size - index - 1);
        element[--size] = null;
        return oldData;
    }

    public E remove() {
        E removeData;
        removeData = remove(size - 1);
        return removeData;
    }

    @SuppressWarnings(&quot;unchecked&quot;)
    public E get(int index) {
        rangeCheck(index);
        return element(index);
    }

    public boolean remove(Object o) {
        if (o == null) {
            for (int i = 0; i &lt; size; i++) {
                if (element[i] == null) {
                    fastRemove(i);
                    return true;
                }
            }
        } else {
            for (int i = 0; i &lt; size; i++) {
                if(element[i].equals(o)){
                    fastRemove(i);
                    return true;
                }
            }
        }
        return false;
    }

    public int size(){
        return this.size;
    }

    public int indexOf(Object o){
        if(o == null){
            for (int index = 0; index &lt; size; index++) {
                if(element[index]==null)
                    return index;
            }
        } else {
            for (int index = 0; index &lt; size; index++) {
                if (element[index].equals(o)) {
                    return index;
                }
            }
        }
        return -1;
    }

    public boolean contains(Object o){
        return indexOf(o) &gt; -1;
    }

    public boolean isEmpty(){
        return size == 0;
    }

    public E set(int index, E e){
        rangeCheck(index);

        E oldData = element(index);
        element[index] = e;
        return oldData;
    }

    public void clear(){
        for (int i = 0; i &lt; size; i++) {
            element[i] = null;
        }
        this.size = 0;
    }

    protected void removeRange(int fromIndex, int toIndex) {
        rangeCheck(fromIndex);
        rangeCheck(toIndex);

        int numMove = size - toIndex ;
        if(numMove &gt; 0) {
            System.arraycopy(element, toIndex, element, fromIndex, numMove);
        }

        int newSize = size - (toIndex - fromIndex);

        for (int i = newSize; i &lt; size; i++) {
            element[i] = null;
        }

        this.size = newSize;

    }

    private void fastRemove(int index) {
        System.arraycopy(element, index + 1, element, index, size - index - 1);
        element[--size] = null;
    }

    private void rangeCheck(int range) {
        if (range &lt; 0 || range &gt; size - 1) {
            throw new ArrayIndexOutOfBoundsException(range);
        }
    }

    public Object[] toArray(){
        return Arrays.copyOf(element, size);
    }

    private void ensureCapacity(int capacity) {
        if (capacity &gt;= this.capacity) {
            this.capacity = (this.capacity * 3) / 2  +1;
            if (this.capacity &lt;= capacity) {
                this.capacity = capacity;
            }
            element = Arrays.copyOf(element, this.capacity);
        }
    }

    public static void main(String[] args) {
        MyArrayList&lt;String&gt; myList = new MyArrayList&lt;String&gt;();

        for (int i = 0; i &lt; 10; i++) {
            myList.add(&quot;hello world&quot;);
        }

        ArrayList&lt;String&gt; addList = new ArrayList&lt;String&gt;(10);
        addList.add(&quot;hello&quot;);
        addList.add(&quot;leilei&quot;);
        addList.add(&quot;leilei2&quot;);
        myList.addAll(addList);
        System.out.println(addList);
        System.out.println(myList);
        System.out.println(myList.indexOf(&quot;leilei&quot;));
        System.out.println(myList.contains(&quot;sf&quot;));
        System.out.println(myList.remove(&quot;leilei&quot;));
        System.out.println(myList);
        System.out.println(myList.indexOf(&quot;leilei&quot;));
        myList.addAll(8, addList);
        System.out.println(myList);
    }

}
[/code]