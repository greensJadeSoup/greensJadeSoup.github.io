## 慢查询
可以记录操作时间大于x的所有记录，在redis.conf中设置，自行百度即可，但不是我想要的
## monitor
可以记录访问的客户端ip，端口，操作，而在java代码中引入JedisMonitor包也可使用，调用方法如下：
```java
 //设置监控
new Thread(new Runnable() {
    public void run() {
        jedis.monitor(new JedisMonitor() {
            @Override
            public void onCommand(String command) {
                System.out.println("#monitor: " + command);
            }
        });
    }
}).start();
```
### 代码
参考一下两个网址
https://blog.csdn.net/cxfly957/article/details/79085785
https://blog.csdn.net/douxingpeng1/article/details/83050716
半实现代码
```java
package utils;

import java.io.*;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.UUID;

public class Analysis {
    public void storage(String command){
        //日期文件路径
        Date date = new Date();
        String filePath="F:/workApp/IdeaProjectsWorkspace/redis_all/redis-monitor/logs/"+new SimpleDateFormat("yyyy/MM/dd/").format(date);
        //日志文件路径
        String analysis = command.substring(command.indexOf("[")+3,command.indexOf("]"))+"_"+new SimpleDateFormat("hh").format(date);
        String logName = analysis + ".log";
        String logPath=filePath+"/"+logName;
        //写入换行定义
        String newline = System.getProperty("line.separator");

        //如果不存在,创建文件夹
        File file = new File(filePath);
        if(!file.exists()){
            file.mkdirs();
            file.mkdirs();
            file = new File(filePath); // 重新实例化
        }
        //如果不存在,创建日志文件
        File logFile = new File(logPath);
        if(!logFile.exists()){
            logFile.mkdirs();
            logFile = new File(logPath); // 重新实例化
        }
        //将monitor读取到的放入日志中
        try {
            FileOutputStream fw = new FileOutputStream(logFile,true);
            Writer out = new OutputStreamWriter(fw, "utf-8");
            out.write(command);
            out.write(newline);
            out.close();
            fw.flush();
            fw.close();
        } catch (Exception ex) {
            System.out.println(ex.getMessage());
        }
        /*OutputStream out;
        UUID uuid = UUID.randomUUID();
        String fileName = uuid + ".log";
        try {
            out = new FileOutputStream(path + fileName);
            out.flush();
            out.close();
        }catch (IOException e) {
            e.printStackTrace();
        }*/

        System.out.println("F:/workApp/IdeaProjectsWorkspace/redis_all/redis-monitor/--is ok");
    }
}

```
## log4j按日期和文件大小切割日志
网址https://blog.csdn.net/zhangyunfeixyz/article/details/79039273
再结合以上monitor监听和动态生成日志文件夹
## 注意点
文件夹不能带":",所以按端口创建文件夹一直失败。。。
>xxxx:8080不能作为文件夹名
## 
