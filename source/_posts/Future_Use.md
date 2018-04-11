---
title: Future Callable 初使用
date: 2016-11-19 14:28:41
tags: 
- Future
- excel导出
categories: 
- 笔记
- Java 
- 多线程
---
 ## 线程优化 
  昨天看到一同事的导出代码，单线程，看着就觉得还是差了点东西。然后就想进行一番改造，然后就找来了future和callable。
  **
      Callable接口类似于Runnable，从名字就可以看出来了，但是Runnable不会返回结果，并且无法抛出返回结果的异常，而Callable功能更强大一些，被线程执行后，可以返回值，这个返回值可以被Future拿到，也就是说，Future可以拿到异步执行任务的返回值。
  **

  简单来说就是一个是执行任务，然后它带着任务执行的结果回来。

### 简单看下下面的demo
```
import java.util.ArrayList;
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class demo{
    public static void main(String[] args) throws InterruptedException, ExecutionException {
        ExecutorService executor = Executors.newCachedThreadPool();
        ArrayList<Future<Integer>> resultList = new ArrayList<>();
        
        //创建并提交任务1
        AddNumberTask task1 = new AddNumberTask(1, 5000);
        Future<Integer> future1 = executor.submit(task1);
        resultList.add(future1);
        
        //创建并提交任务2
        AddNumberTask task2 = new AddNumberTask(5001, 10000);
        Future<Integer> future2 = executor.submit(task2);
        resultList.add(future2);
        
        executor.shutdown();
        
        int total = 0;
        
        for(Future<Integer> future : resultList){
            while(true){
                if(future.isDone() && !future.isCancelled()){
                    int sum = future.get();
                    total += sum;
                    break;
                }
                else{
                    Thread.sleep(100);
                }
            }
        }
        
        System.out.println("total sum is " + total);
    }

}

class AddNumberTask implements Callable<Integer>{
    private int start;
    private int end;
    
    public AddNumberTask(int start, int end) {
        // TODO Auto-generated constructor stub
        this.start = start;
        this.end = end;
    }
    
    @Override
    public Integer call() throws Exception {
        // TODO Auto-generated method stub
        int totalSum = 0;
        
        for(int i = start; i <= end; i++){
            totalSum += i;
        }
        
        Thread.sleep(5000);
        
        return totalSum;
    }
    
}

```
- 参考了这个例子，我就改造了导出的那段代码，1w2数据从原来的6-8s 到现在的2-3s，虽然不是很多，但是也是一大进步，后来想了下，时间其实就花在了查询的sql，如果sql 再继续优化下去的的话应该可以突破到1s执行返回的。但因为还有新需求要进行，我就不再细究下去了。

- 改造的时候，同样我也是根据查询的数据量来新建任务执行，使用了for循环，本来公司已经提供了线程池fixedPool(100)的，我就不用自己再创建了。

### 出现的问题
- future 没有表示返回来的这堆数据是谁的
  于是我就对返回变量进行了修改，返回一个map，标识这个数据是我的，后面根据map的key获取数据。
- size 数据量的大小影响任务的执行
  查询数据使用了limit 字段每次查询1000 2000 3000 500 这几个size 都试过，似乎值越大越好，一次查询数据量比多次查询数据量效率要高。因为3000恐怕生产环境的数据会比较多，后来我就改成了2000。  
- mysql 索引 不合理
  索引建不好，导致查询sql也是很慢的，查询的那个表使用了多列索引，问题就出在这了，使用sql的执行计划看了下，查询的字段虽然有索引但是没有使用到，因为多列索引的第一个不是我的查询字段，导致查询效率会有小小的降低，我也没跟负责人说，毕竟也不是很大的问题，差的时间不是很多。