# AtomicBoolean 

AtomicBoolean 这个类，他是线程安全的，它的compareAndSet方法是利用CAS来实现线程安全的，

它的两种用法：

1.保证某段语句只执行一次。 执行结束后不修改回false

首先我们要知道compareAndSet的作用，判断对象当时内部值是否为第一个参数，如果是则更新为第二个参数，且返回ture，否则返回false。那么默认初始化为false，则一个线程把他变为ture，compareAndSet返回ture，进入方法体执行逻辑，那么其他的任何线程进入该方法执行compareAndSet时第一个参数为false，而对象的内部值已经被修改为true，则永远过不了if。

    //创建新对象，提供默认值false
    private static AtomicBoolean initialized = new AtomicBoolean(false);  
     
    public void init()
    {   
        //第一个参数与默认值一致，把值改为第二个参数，返回true  后面在执行则都是false。
       if( initialized.compareAndSet(false, true) )
       {
           // 这里放置初始化代码....
       }
    }


​       
2.保证该方法只有一个线程执行，类似于锁的操作 执行结束后修改回false

方法只执行1次，那么方法执行结束的时候我把对象的值变成false，compareAndSet可以继续可以将对象则的值设置成true，返回true。
    
    @Override    
     public void run() {     
         if (exists.compareAndSet(false, true)) {    
             
             exists.set(false);      
        } else {    
             System.out.println(name + " give up");    
        }    
    
    }  