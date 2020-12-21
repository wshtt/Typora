# redis 分布式锁

        锁是针对某个资源，保证其访问的互斥性，在实际使用当中，这个资源一般是一个字符串。
        使用Redis 实现锁，主要是将资源放到 Redis 当中，利用其原子性，当其他线程访问时，如果 Redis 中已经存在这个资源，就不允许之后的一些操作。
        spring boot使用 Redis 的操作主要是通过 RedisTemplate 来实现，一般步骤如下：
    
        //1.  使用 Redis 的 setIfAbsent 方法（如果 key 不存在则存放，存在则不存放 返回 true or false）
              redisTemplate.opsForValue().setIfAbsent("key", "value");
        //2.  设置过期时间
              redisTemplate.expire("key", 30000, TimeUnit.MILLISECONDS);
        //3.  释放锁
              redisTemplate.delete("key");
    
        //Redis 2.1以后 ,直接设置过期时间。
              redisTemplate.opsForValue().setIfAbsent("key", "value",long timeout, TimeUnit unit);
              redisTemplate.delete("key");


          redis分布式锁
    
          public boolean getLock(String lockId, long millisecond){
            Boolean look = redisTemplate.opsForValue().setIfAbsent(lockId, "look", millisecond, TimeUnit.MILLISECONDS);
            return look != null && look;
          }
    
          使用
    
          public Result acceptInvite(InviteApplyOrderEntity entity) {
              //获取锁
              boolean lock = redisUtils.getLock(OrderLockEnums.acceptInvite.name(), OrderLockEnums.acceptInvite.getMillisecond());
              if (!lock) {
                  try {
                      Thread.sleep(500);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  acceptInvite(entity);
              }
              //业务代码。。。。。
              //删除锁
              redisUtils.delete(OrderLockEnums.acceptInvite.name());
          }