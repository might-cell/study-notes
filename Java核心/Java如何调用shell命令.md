### Java如何调用shell命令

Java语言可以使用Java Runtime类和ProcessBuilder类来调用shell命令。

使用Runtime类调用shell命令的示例代码如下：

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class ShellCommand {

    public static void main(String[] args) throws Exception {

        // 定义要执行的命令
        String command = "ls -l";

        // 执行命令
        Process process = Runtime.getRuntime().exec(command);

        // 读取命令执行结果
        BufferedReader reader = new BufferedReader(new InputStreamReader(process.getInputStream()));
        String line;
        while ((line = reader.readLine()) != null) {
            System.out.println(line);
        }

        // 关闭命令执行结果读取器
        reader.close();
    }
}

```



使用ProcessBuilder类调用shell命令的示例代码如下：

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;

public class ShellCommand {

    public static void main(String[] args) throws Exception {

        // 定义要执行的命令
        List<String> command = new ArrayList<>();
        command.add("ls");
        command.add("-l");

        // 创建ProcessBuilder对象并设置要执行的命令
        ProcessBuilder builder = new ProcessBuilder(command);
        
        // 执行命令
        Process process = builder.start();

        // 读取命令执行结果
        BufferedReader reader = new BufferedReader(new InputStreamReader(process.getInputStream()));
        String line;
        while ((line = reader.readLine()) != null) {
            System.out.println(line);
        }

        // 关闭命令执行结果读取器
        reader.close();
    }
}

```

以上代码可以执行Linux系统下的"ls -l"命令，并将结果输出到控制台中。需要注意的是，在执行命令时，需要确保Java应用程序具有足够的权限来执行命令。