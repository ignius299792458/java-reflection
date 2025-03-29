# Java Development Environment Components: JVM, JDK, and JRE

When you're developing a Spring application, these three components work together to support your Java development:

## Component Relationships

**JDK (Java Development Kit)** contains:

- Compiler (javac)
- Development tools
- JRE
- Core libraries

**JRE (Java Runtime Environment)** contains:

- JVM
- Core libraries
- Runtime libraries

**JVM (Java Virtual Machine)** is:

- The execution engine that runs your compiled Java bytecode
- Contained within the JRE
- Platform-specific (different implementations for Windows, macOS, Linux)

## JVM Location on Your Device

When you install the JDK for Spring development:

1. The JDK installation creates a directory structure (typically in Program Files on Windows, /usr/lib/jvm on Linux, or /Library/Java on macOS)

2. The JVM itself is implemented as native executable files:
   - Windows: `jvm.dll` in `[JDK_HOME]/jre/bin/server/`
   - Linux: `libjvm.so` in `/usr/lib/jvm/[jdk-version]/jre/lib/[architecture]/server/`
   - macOS: `libjvm.dylib` in `/Library/Java/JavaVirtualMachines/[jdk-version]/Contents/Home/jre/lib/server/`

## JVM Management

In your Spring application:

1. The JVM starts when you run your application (using `java -jar yourapp.jar` or via your IDE)

2. You can configure JVM behavior with command-line options:

   - Memory allocation: `-Xmx2g` (sets max heap size to 2GB)
   - Garbage collection strategy: `-XX:+UseG1GC` (uses G1 garbage collector)
   - System properties: `-Dspring.profiles.active=dev`

3. You can monitor and manage the running JVM using tools:
   - JConsole or VisualVM (included in JDK)
   - Java Mission Control (JMC)
   - Spring Boot Actuator for application-specific metrics

The JVM creates and manages memory spaces (heap, stack, method area) and runs your compiled Java bytecode in a platform-independent way, which is why Java can run on different operating systems.
