# getter 和 setter Boolean问题



JavaBean规范：如果一个属性是boolean值，假设名为property，则其setter方法应该是setProperty，其getter方法应该为isProperty

直接使用注解对`is`开头的boolean 类型时不能正确获取getter 和 setter 方法的，可以手动修改，或者修改变量名

```java
package com.ndp.fb.rdb.model;
 
import com.alibaba.fastjson.JSON;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
 
@Getter
@Setter
@NoArgsConstructor
public classGetterSetterTester{
    private boolean busy;
    private boolean isComplete;
 
    private Boolean verify;
    private Boolean isFinish;
 
    @Override
    public String toString(){
        return JSON.toJSONString(this);
    }
 
    publicstaticvoidmain(String[] args){
        GetterSetterTester t = getterSetter();
        testJson(t);
    }
 
    privatestatic GetterSetterTester getterSetter(){
        GetterSetterTester t = new GetterSetterTester();
        //基本类型：无论带不带is，都是isXxx,setXxx。   sun的标准，需要特殊注意
        t.isBusy();
        t.setBusy(true);
        t.isComplete();
        t.setComplete(true);
 
        //包装类型:和其他类型一样，getXxx,setXxx;(其他类型的isXxx也是 getIsXxx,setIsXxx,不算特例).
        t.getVerify();
        t.setVerify(true);
        t.getIsFinish();
        t.setIsFinish(true);
        return t;
    }
 
    privatestaticvoidtestJson(GetterSetterTester t){
        //期望的json格式
        String json = "{\"busy\":true,\"isComplete\":true,\"isFinish\":true,\"verify\":true}";
        GetterSetterTester p = JSON.parseObject(json, GetterSetterTester.class);
        System.out.println(p.isBusy());
        System.out.println(p.isComplete());  //false
        System.out.println(p.getVerify());
        System.out.println(p.getIsFinish());
        //实际的json格式，可成功反序列化成对象。
        //基本类型变量无论是否有is，json都没有is（与getter一致）。
        System.out.println(t.toString());//{"busy":true,"complete":true,"isFinish":true,"verify":true}
 
        //空对象时的json，基本类型默认是false，会输出；包装类型默认是null，不输出。
        System.out.println(new GetterSetterTester().toString());//{"busy":false,"complete":false}
    }
 
}
```



##### 一、



```java
package com.example.demo01.qianji.entity;

/**
 * @author: wushutong
 * @date: 2020/10/10 18:39
 */
public class BooleanTest {
    private boolean a;
    private boolean isB;
    
    private Boolean c;
    private Boolean isD;

    public boolean isA() {
        return a;
    }

    public void setA(boolean a) {
        this.a = a;
    }

    public boolean isB() {
        return isB;
    }

    public void setB(boolean b) {
        isB = b;
    }

    public Boolean getC() {
        return c;
    }

    public void setC(Boolean c) {
        this.c = c;
    }

    public Boolean getD() {
        return isD;
    }

    public void setD(Boolean d) {
        isD = d;
    }
}
```

