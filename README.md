# hsdis
Hotspot disassembler extracted from openjdk.

# Precondition on linux
To build hsdis on linux you need to make sure you have the standard build tools
```
apt-get install build-essential
```

# Usage
```
git clone https://github.com/liuzhengyang/hsdis
cd hsdis
tar -zxvf binutils-2.26.tar.gz
make BINUTILS=binutils-2.26 ARCH=amd64
```
And then copy hsdis build file to the target folder in JDK.

### OSX
```
sudo cp build/macosx-amd64/hsdis-amd64.dylib /Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/server/
```
### Linux
```
sudo cp build/linux-amd64/hsdis-amd64.so /usr/lib/jvm/java-8-oracle/jre/lib/amd64/server/
```

After that, you could add `-XX:+UnlockDiagnosticVMOptions -XX:+PrintAssembly ` in the Java Run Param to see the program's assebmle code.
One Example:
```
java -XX:+UnlockDiagnosticVMOptions -XX:+PrintAssembly -Xcomp -XX:CompileCommand=compileonly,*VolatileTest.main com.io.lzy.VolatileTest

```

下面会用到的 java 命令参数的解释，参考 [Java Platform, Standard Edition Tools Reference#java：](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html)

```
-XX:+UnlockDiagnosticVMOptions：解锁用于 JVM 诊断的选项。
-XX:+PrintAssembly：配合反汇编插件（例如 hsdis-amd64.dylib）可以打印出字节码和本地方法的汇编码；必须和 -XX:+UnlockDiagnosticVMOptions 一起使用。
-Xcomp：在第一次调用时强制编译方法。默认情况下，无论是 -client 模式还是 -server 模式，都需要执行一定次数解释方法的调用才会触发方法的编译。（如果需要 JIT 日志，则不指定该参数）
-XX:CompileCommand=compileonly,*ClassName.methodName：只编译类名为 ClassName 中的 methodName 方法，支持使用 * 作为通配符。可以多次指定 -XX:CompileCommand 添加多条命令。（建议只指定需要的方法，否则将会产生大量的无关日志）

-XX:+LogCompilation：允许将编译活动记录到当前工作目录中名为 hotspot.log 的文件中。可以通过 -XX:LogFile 指定文件的路径和名字。
-XX:LogFile=path：指定日志的路径和文件名。例如：-XX:LogFile=/var/log/hotspot.log
```

There is already a prebuild hsdis-amd64 for OSX 64 in build/macosx-amd64/macosx-amd64.dylib and for linux in build/linux-amd64/hsdis-amd64.so and was build for/with Unbuntu 16.04
