# log4j

log4j是一个开源的日志，共分为六个等级：LOG、DEBUG、INFO、WARN、ERROR、和FATAL。


      if(log.isDebugEnabled()){
         log.debug("debug！"); 
      } 


​      
在我们开发阶段有时候需要查看特定数据，我们可以把日志级别定为DEBUG级，调试信息会输出在日志里便于调试和跟踪修改bug。当产品发布上线之后，可以在log4j配置中去掉DEBUG级别，这时调试信息就不会输出在日志里，日志会只显示运行的相关信息。如此一来，控制输出什么日志不需要修改代码，只需修改配置文件的参数而已。