

##### 一、开启新线程，消费队列



###### 调用处

```java
// 设置map
Map<String, Object> map = new HashMap<>();
map.put("accessLog",accessLog);
map.put("number",0);

// 放入 AccessLogUpdateServiceImpl 的静态变量 accessLogList
AccessLogUpdateServiceImpl.accessLogList.add(map);
if (!AccessLogUpdateServiceImpl.accessLogFlag){
    // 如果方法没有在执行，则调用方法
    accessLogUpdateService.asyncAccessLogUpdate();
}
```

###### service

```java
package com.qianji.mongo.service;

/**
 * @author: wushutong
 * @date: 2020/12/29 14:45
 */
public interface AccessLogUpdateService {

    // 执行设置 parkEntity 到 mango的 accessLog
    void asyncAccessLogUpdate();
}

```

###### serviceImpl

```java
package com.qianji.mongo.service.impl;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.core.toolkit.Wrappers;
import com.qianji.entity.CarOrderFormEntity;
import com.qianji.entity.MerchantParkEntity;
import com.qianji.mongo.entity.AccessLog;
import com.qianji.mongo.service.AccessLogUpdateService;
import com.qianji.service.CarOrderFormService;
import com.qianji.service.MerchantParkService;
import org.apache.commons.lang3.StringUtils;
import org.apache.commons.lang3.concurrent.BasicThreadFactory;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.mongodb.core.MongoTemplate;
import org.springframework.data.mongodb.core.query.Criteria;
import org.springframework.data.mongodb.core.query.Query;
import org.springframework.data.mongodb.core.query.Update;
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.ScheduledThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

/**
 * @author: wushutong
 * @date: 2020/12/29 14:47
 */
@Service
public class AccessLogUpdateServiceImpl implements AccessLogUpdateService {

    // 静态变量
    public static List<Map<String,Object>> accessLogList = new ArrayList<>();
    public static boolean accessLogFlag = false;
	
    // 日志
    private static final Logger logger = LoggerFactory.getLogger(AccessLogUpdateServiceImpl.class);
    @Autowired
    private CarOrderFormService carOrderFormService;
    @Autowired
    private MerchantParkService merchantParkService;
    @Autowired
    private MongoTemplate mongoTemplate;
	// 开辟线程
    private ScheduledExecutorService scheduledService = new ScheduledThreadPoolExecutor(1,
            new BasicThreadFactory.Builder().namingPattern("access-log-update").daemon(true).build());

    // 外部调用的方法
    @Override
    public void asyncAccessLogUpdate(){
        // 执行 -> 方法
        scheduledService.execute(() -> asyncSleep());
    }

    // while（true）方法
    private void asyncSleep(){
        accessLogFlag = true;
        while (accessLogList.size() > 0){
            try {
                Thread.sleep(10000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            this.setParkEntity();
        }
        accessLogFlag = false;
    }

    // 业务
    private void setParkEntity() {
        // 遍历设置parkEntity
        for (int i = accessLogList.size()-1; i >= 0; i--) {
            Map<String, Object> map = accessLogList.get(i);
            Object log = map.get("accessLog");

            if (log == null){
                accessLogList.remove(i);
                continue;
            }
            AccessLog accessLog = (AccessLog) log;

            // 查询车辆
            LambdaQueryWrapper<CarOrderFormEntity> wrapper = Wrappers.lambdaQuery();
            wrapper.eq(StringUtils.isNotBlank(accessLog.getCar_no()),CarOrderFormEntity::getCarNum,accessLog.getCar_no());
            wrapper.eq(StringUtils.isNotBlank(accessLog.getDevice_sn()),CarOrderFormEntity::getDeviceSno,accessLog.getDevice_sn());
            wrapper.eq(CarOrderFormEntity::getStartTime,accessLog.getSys_time());
            List<CarOrderFormEntity> list = carOrderFormService.list(wrapper);
            // 查不到订单
            if (list == null && list.size() <= 0){
                Integer number = (Integer) map.get("number");
                if (number >= 9){
                    logger.info("车辆进出日志匹配临时车parkId长时间查询不到订单：" + log);
                    accessLogList.remove(i);
                }else {
                    number++;
                    map.put("number",number);
                }
                continue;
            }
            // 订单过多
            if (list.size() > 1){
                logger.info("查询到的重复订单"+ list.toString());
                accessLogList.remove(i);
                continue;
            }

            MerchantParkEntity parkEntity = merchantParkService.getOne(
                    new LambdaQueryWrapper<MerchantParkEntity>().
                            eq(MerchantParkEntity::getParkCode, list.get(0).getParkCode()));

            if (parkEntity == null){
                logger.info("查询到的此车位"+ list.get(0).getParkCode());
                accessLogList.remove(i);
                continue;
            }

            // 修改
            Query query = new Query(Criteria.where("_id").is(accessLog.getId()));
            Update update = new Update();
            update.set("parkEntity",parkEntity);
            mongoTemplate.updateFirst(query, update, AccessLog.class);
            // 删除
            accessLogList.remove(i);
        }
    }

    
    
    
    // 以下是测试使用的，和上面一样。
    public static List<Map<String,Integer>> mapList = new ArrayList<>();
    public static boolean intFlag = false;

    @Override
    public void test() {
        scheduledService.execute(() -> test1());
    }

    private void test1(){
        intFlag = true;
        logger.info("执行的订单" + mapList.toString());
        logger.info("时间：" + System.currentTimeMillis());
        while (mapList.size() > 0){
            try {
                Thread.sleep(10000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            this.test2();
        }
        intFlag = false;
        logger.info("执行的订单" + mapList.toString());
        logger.info("时间：" + System.currentTimeMillis());
    }

    private void test2(){
        logger.info("test2开始执行的订单" + mapList.toString());
        logger.info("test2开始时间：" + System.currentTimeMillis());
        for (int i = mapList.size()-1; i >= 0; i--) {
            Map<String, Integer> map = mapList.get(i);
            Integer integer = map.get("accessLog");
            if (integer == 3){
                logger.info("删除：33333333");
                System.out.println("3");
                mapList.remove(i);
            }else {
                Integer number = map.get("number");
                if (number >= 9){
                    mapList.remove(i);
                    logger.info("次数太多");
                }else {
                    number++;
                    map.put("number",number);
                }
            }
        }
        logger.info("test2结束执行的订单" + mapList.toString());
        logger.info("test2结束时间：" + System.currentTimeMillis());
    }
}

```

