## 集合
### 1、Collection接口
```
//接口中有两个基本方法
public interface Collection<E>
{
    //在集合尾部添加元素，若改变了集合，返回true;没有变化，返回false
    boolean add(E element);  

    //返回一个实现了Iterator接口的对象，用这个迭代器对象来依次访问集合中的元素    
    Iterator<E> iterator();      
    ...
}
```    
### 2、迭代器  
```
public interface Iterator<E>
{
    //反复调用，逐个访问集合中的元素，若到达末尾，方法抛出一个NoSuchElementException
    E next();

    //若还有元素未访问，返回true，在next()方法之前使用             
    boolean hasNext();

    //删除上次调用next方法返回的元素
    void remove();
    
    //提供一个遍历集合元素的方法
    default void forEachRemaining(Consumer<? super E> action);
}
```  
+ **遍历集合的三种方法：**  
   1. 请求一个迭代器，在hasNext返回true时反复调用next方法，例：  
   ```
   Collection<String> coll = ...;
   Iterator<String> iter = coll.iterator();
   while(iter.hasNext())
   {
       String element = iter.next();
       ...
   }
   ```
   2. 使用“for each”循环  
   ```
   for(String element : coll)
   {
       ...
   }
   ```  
    + 注：for each循环可以遍历任何实现了Iterable接口的对象，Collection接口扩展了Iterable接口，标准类库中任何集合都可以使用for each循环。    
        ```
            public interface Iterable<E>
            {
                Iterator<E> iterator();
                ...
            }
        ```   
   3. 调用forEachRemaining方法   


   `iterator.forEachRemaining(element -> do something with element);`  


+  元素访问顺序取决于集合类型  
+ 迭代器认为是介于两个元素之间的，当调用next时，迭代器就越过下一个元素，并返回刚刚越过的那个元素的引用，remove可删除越过的元素。
+ 对集合遍历过一遍之后，再从头开始访问需要再引用一个新的迭代器  
```
    Collection<String> coll = ...;
    Iterator<String> iter = coll.iterator();
    ...
    iter = coll.iterator();      //迭代器位置回到了起点
```
### 3、Java集合框架
1. Java集合框架图  
![](https://github.com/xishi-huan/Java-Learning/blob/master/img-folder/collection.jpg)
2. 接口与实现类  
   + 接口：List-有序且允许元素重复，实现类： ArrayList、LinkedList、Vector
   + 接口：Set-不允许元素重复，实现类：HashSet、TreeSet
   + 接口：Map ，实现类：HashMap、TreeMap
   + 接口：Deque，实现类：ArrayDeque、LinkedList

