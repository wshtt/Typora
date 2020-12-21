# RandomStringUtils


        Random object used by random method. This has to be not local to the random method so as to not return the same value in the same millisecond
        随机方法使用的随机对象。这必须不是局部的返回到随机方法，以便不返回相同的值同样的毫秒
        
        1. 
            public static String random(int count)
              创建一个随机字符串，其长度是指定的字符数。
              将从所有字符集中选择字符(中文环境乱码)
              RandomStringUtils.random(15)
        2.
            public static String random(int count, boolean letters, boolean numbers)
              count 创建一个随机字符串，其长度是指定的字符数,字符将从参数的字母数字字符集中选择，如参数所示。
              letters true,生成的字符串可以包括字母字符
              numbers true,生成的字符串可以包含数字字符
              （两个全部为false 则从所有字符集选择，中文环境乱码）
              RandomStringUtils.random(15, true, false)
        3. 
            public static String random(int count, String chars)
              创建一个随机字符串，其长度是指定的字符数。
              字符将从字符串指定的字符集中选择，不能为空。如果NULL，则使用所有字符集。
              RandomStringUtils.random(15,"123abc")
        4.
            public static String random(int count, int start,int end,boolean letters, boolean numbers)
              创建一个随机字符串，其长度是指定的字符数,字符将从参数的字母数字字符集中选择，如参数所示。
              count:计算创建的随机字符长度
              start:字符集在开始时的位置
              end:字符集在结束前的位置，必须大于65
              letters true,生成的字符串可以包括字母字符
              numbers true,生成的字符串可以包含数字字符
              RandomStringUtils.random(1009, 5, 129, true, true)
        5. 
            public static String randomAlphabetic(int count)
              产生一个长度为指定的随机字符串的字符数，字符将从拉丁字母（a-z、A-Z的选择）。
              count:创建随机字符串的长度
              RandomStringUtils.randomAlphabetic(15)
        6.
            public static String randomAlphabetic(int minLengthInclusive, int maxLengthExclusive)
              创建一个随机字符串，其长度介于包含最小值和最大最大值之间,，字符将从拉丁字母（a-z、A-Z的选择）。
              minLengthInclusive ：要生成的字符串的包含最小长度
              maxLengthExclusive ：要生成的字符串的包含最大长度
              RandomStringUtils.randomAlphabetic(2, 15)
        7. 
            public static String randomAlphanumeric(int count)
              创建一个随机字符串，其长度是指定的字符数，字符将从拉丁字母（a-z、A-Z）和数字0-9中选择。
              count ：创建的随机数长度
              RandomStringUtils.randomAlphanumeric(5)
            
        8.
            public static String randomAlphanumeric(int minLengthInclusive,int maxLengthExclusive)
              创建一个随机字符串，其长度介于包含最小值和最大最大值之间,字符将从拉丁字母（a-z、A-Z）和数字0-9中选择。
              minLengthInclusive ：要生成的字符串的包含最小长度
              maxLengthExclusive ：要生成的字符串的包含最大长度
              RandomStringUtils.randomAlphanumeric(2,5)
        
        9. 
            public static String randomAscii(int count)
              创建一个随机字符串，其长度是指定的字符数，字符将从ASCII值介于32到126之间的字符集中选择（包括）
              count:随机字符串的长度
              RandomStringUtils.randomAscii(15)
        10.
            public static String randomAscii(int minLengthInclusive, int maxLengthExclusive)
              创建一个随机字符串，其长度介于包含最小值和最大最大值之间,字符将从ASCII值介于32到126之间的字符集中选择（包括）
              minLengthInclusive ：要生成的字符串的包含最小长度
              maxLengthExclusive ：要生成的字符串的包含最大长度
              RandomStringUtils.randomAscii(2, 15)
        11. 
            public static String randomNumeric(int count)
              创建一个随机字符串，其长度是指定的字符数，将从数字字符集中选择字符。
              count ：创建的随机数长度
              RandomStringUtils.randomNumeric(5)
            
        12.
            public static String randomNumeric(int minLengthInclusive,int maxLengthExclusive)
              创建一个随机字符串，其长度是指定的字符数，将从数字字符集中选择字符。
              minLengthInclusive ：要生成的字符串的包含最小长度
              maxLengthExclusive ：要生成的字符串的包含最大长度
              RandomStringUtils.randomNumeric(2,5)


​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​        
​            