# PriorityQueue In Java

## 1. PriorityQueue与Queue的区别

* **Queue** 先进先出（FIFO）的队列

  严格按照FIFO取出队首元素，无法满足插队需求

* **PriorityQueue** 的出队顺序与元素的优先级有关，对其调用remove()方法或者poll()方法，返回的总是优先级最高的元素。

  * 优先队列的大小不受限制，但在创建时可以指定初始大小。当我们向优先队列增加元素的时候，队列大小会自动增加
  * 它是**非线程安全**的，所以Java也提供了**PriorityBlockingQueue**（实现BlockingQueue接口）用于Java多线程环境
  * 它是基于优先级堆的无界优先级队列。优先级队列的元素按照它们的 自然顺序排序，或者Comparator按照队列构造时提供的顺序排序，具体取决于使用的构造函数。
  * 优先级队列**不允许null**元素。依赖于自然排序的优先级队列也不允许插入不可比较的对象（这样做可能会导致 `ClassCastException`）。

  ~~~java
  import java.util.PriorityQueue;
  import java.util.Queue;
  public class Main {
      public static void main(String[] args) {
          Queue<String> q = new PriorityQueue<>();
          //add elements to the queue
          q.offer("apple");
          q.offer("pear");
          q.offer("banana");
          System.out.println(q.poll()); // apple
          System.out.println(q.poll()); // banana!!!! not pear
        //The reason is the ordering of the strings, banana has more priority than pear.
          System.out.println(q.poll()); // pear
          System.out.println(q.poll()); // null
      }
  }
  ~~~

## 2.实现Comparable接口，构建PriorityQueue优先级

* 如果放入PriorityQueue中的元素实现了Comparable接口，PriorityQueue就会根据元素的排序顺序决定出队的优先级。

* 如果放入的元素并没有实现Comparable接口,我们就需要提供一个Comparator对象来判断两个元素的顺序。

  ~~~java
  import java.util.Comparator;
  import java.util.PriorityQueue;
  import java.util.Queue;
  
  public class test {
      public static void main(String[] args) {
          Queue<User> q = new PriorityQueue<>(new UserComparator());
          // 添加3个元素到队列:
          q.offer(new User("staff1", "A1"));
          q.offer(new User("staff2", "A2"));
          q.offer(new User("Boss", "V1"));
          System.out.println(q.poll()); // Boss/V1
          System.out.println(q.poll()); // staff1/A1
          System.out.println(q.poll()); // staff2/A2
          System.out.println(q.poll()); // null,因为队列为空
      }
      //提供一个Comparator对象来判断两个元素的顺序
      static class UserComparator implements Comparator<User> {
          public int compare(User u1, User u2) {
              if (u1.number.charAt(0) == u2.number.charAt(0)) {
                  // 如果两人的号都是A开头或者都是V开头,比较号的大小:
                  return u1.number.compareTo(u2.number);
              }
              if (u1.number.charAt(0) == 'V') {
                  // u1的号码是V开头,优先级高:
                  return -1;
              } else {
                  return 1;
              }
          }
      }
  
      static class User {
          public final String name;
          public final String number;
  
          public User(String name, String number) {
              this.name = name;
              this.number = number;
          }
  
          public String toString() {
              return name + "/" + number;
          }
      }
  }
  ~~~

## 3. PriorityQueue底层实现原理

* PriorityQueue通过堆实现，具体说是通过完全二叉树（*complete binary tree*）实现的**小顶堆** *（任意一个非叶子节点的权值，都不大于其左右子节点的权值）*
* 这也就意味着可以通过数组来作为*PriorityQueue*的底层实现。

<img src="/Users/white/Library/Application Support/typora-user-images/image-20220101160453233.png" alt="image-20220101160453233" style="zoom:33%;" />

* 参考https://blog.csdn.net/u010623927/article/details/87179364 

## 4. leetcode中的一些使用 待补充

