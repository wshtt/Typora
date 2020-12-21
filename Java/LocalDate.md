# LocalDate


​           
```java
       // 1. LocalDate是日期处理类，具体API如下：

        // 获取当前日期
        LocalDate now = LocalDate.now();
        // 设置日期
        LocalDate localDate = LocalDate.of(2019, 9, 10);
        // 获取年
        int year = localDate.getYear();     //结果：2019
        int year1 = localDate.get(ChronoField.YEAR); //结果：2019
        // 获取月
        Month month = localDate.getMonth();   // 结果：SEPTEMBER
        int month1 = localDate.get(ChronoField.MONTH_OF_YEAR); //结果：9
        // 获取日
        int day = localDate.getDayOfMonth();   //结果：10
        int day1 = localDate.get(ChronoField.DAY_OF_MONTH); // 结果：10
        // 获取星期
        DayOfWeek dayOfWeek = localDate.getDayOfWeek();   //结果：TUESDAY
        int dayOfWeek1 = localDate.get(ChronoField.DAY_OF_WEEK); //结果：2

        // 2. LocalTime是时间处理类，具体API如下：

        复制代码
        // 获取当前时间
        LocalTime now = LocalTime.now();
        // 设置时间
        LocalTime localTime = LocalTime.of(13, 51, 10);
        //获取小时
        int hour = localTime.getHour();    // 结果：13
        int hour1 = localTime.get(ChronoField.HOUR_OF_DAY); // 结果：13
        //获取分
        int minute = localTime.getMinute();  // 结果：51
        int minute1 = localTime.get(ChronoField.MINUTE_OF_HOUR); // 结果：51
        //获取秒
        int second = localTime.getSecond();   // 结果：10
        int second1 = localTime.get(ChronoField.SECOND_OF_MINUTE); // 结果：10
        复制代码

        // 3. LocalDateTime可以设置年月日时分秒，相当于LocalDate + LocalTime

        复制代码
        // 获取当前日期时间
        LocalDateTime localDateTime = LocalDateTime.now();
        // 设置日期
        LocalDateTime localDateTime1 = LocalDateTime.of(2019, Month.SEPTEMBER, 10, 14, 46, 56);
        LocalDateTime localDateTime2 = LocalDateTime.of(localDate, localTime);
        LocalDateTime localDateTime3 = localDate.atTime(localTime);
        LocalDateTime localDateTime4 = localTime.atDate(localDate);
        // 获取LocalDate
        LocalDate localDate2 = localDateTime.toLocalDate();
        // 获取LocalTime
        LocalTime localTime2 = localDateTime.toLocalTime();
```



```java
        //标记当前时区的本地时间
                LocalDate orderTime = orderStartTime.toInstant().atZone(ZoneId.systemDefault()).toLocalDate();

        // 4. Date转成LocalDateTime
        new Date().toInstant().atZone(ZoneId.systemDefault()).toLocalDateTime()

          Date date = new Date();
          Instant instant = date.toInstant();
          ZoneId zoneId = ZoneId.systemDefault();

          LocalDateTime localDateTime = instant.atZone(zoneId).toLocalDateTime();
```