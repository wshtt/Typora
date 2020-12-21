# lambada 表达式


lambada 表达式实质上是一个匿名方法，但该方法并非独立执行，而是用于实现由函数式接口定义的唯一抽象方法

函数式接口：
    
    函数式接口是仅含一个抽象方法的接口，但可以指定 Object 定义的任何公有方法。
    @FunctionalInterface   标识函数式接口（可有可无）
    public interface IFunctionMulti<T extends Number> {
        void multi(List<T> numbers); // 抽象方法
    
        boolean equals(Object obj);  // Object中的方法
    }

lambda表达式：

    单独表达式
    list.forEach(item -> System.out.println(item));
    
    包含代码块
    list.forEach(item -> {
      int numA = item.getNumA();
      int numB = item.getNumB();
        System.out.println(numA + numB);
    });