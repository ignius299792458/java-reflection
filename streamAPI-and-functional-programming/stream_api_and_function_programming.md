# Stream API and Functional Programming in Java

## Simple Introduction

**`Functional Programming: `** in `Java lets you treat functions as values` that **can be passed around, similar to how you pass variables**. This makes code more concise and often easier to understand.

**`Lambda Expressions: `** are like `mini-functions you can write directly in your code without naming them`. Think of them as shorthand methods.

**`Method References`** are even shorter ways to refer to methods that already exist.

**`Functional Interfaces`** are special interfaces with exactly one abstract method, which makes them compatible with lambdas.

**`Stream API`** lets you process collections of data in a pipeline style, focusing on what you want to do rather than how to do it: `streams` and `parallelStreams`

# Functional Interfaces & Lambda Expressions

### How It Works:

When you write a lambda expression like:

```java
Runnable r = () -> System.out.println("Hello");
```

At the JVM level, several things happen:

1. **`Bytecode Generation`**: The Java compiler doesn't create a separate class file for the lambda. Instead, it generates a special method called a "**`lambda factory`**" within your class.

2. **`Invokedynamic`**: The compiler uses the **invokedynamic** instruction, introduced in Java 7, to delay the decision on how to implement the lambda until runtime.

3. **`Lambda Metafactory`**: At runtime, the JVM calls `LambdaMetafactory.metafactory()` which dynamically creates an implementation of the functional interface.

4. **`Implementation Options`**: Depending on the situation, the JVM might:
   - Create a new anonymous class instance (similar to pre-Java 8)
   - Use a method handle directly
   - Generate a specialized class dynamically

### Example:

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.forEach(name -> System.out.println(name));
```

At the machine level:

- The lambda becomes a method in your class
- The JVM uses method handles (direct pointers to methods) for efficient invocation
- No anonymous class instances are created per iteration, reducing memory overhead

# Method References

Method references like `System.out::println` are even more efficient than lambdas:

1. **Direct Linking**: The JVM can directly link to the target method
2. **No Intermediate Code**: No wrapper code is needed
3. **Potential Inlining**: The JIT compiler may inline these calls for better performance

# Stream API

### Architecture:

The Stream API consists of:

1. **Source Operations**: Create a stream from a collection or other source
2. **Intermediate Operations**: Transform the stream (filter, map, etc.)
3. **Terminal Operations**: Produce a result (collect, reduce, forEach)

### Machine-Level Implementation:

1. **Lazy Evaluation**:

   - Intermediate operations only build a recipe
   - No computation happens until a terminal operation is called
   - This allows the JVM to optimize the entire pipeline

2. **Spliterator Mechanism**:

   - Splits collections for parallel processing
   - Handles the complexity of dividing work

3. **Fork/Join Framework**:
   - For parallel streams, the work is distributed across the ForkJoinPool
   - Uses a work-stealing algorithm where idle threads can take tasks from busy ones

### Memory Optimization:

1. **Loop Fusion**: The JVM combines multiple operations into a single pass
2. **Short-Circuiting**: Operations like `findFirst()` or `limit()` allow the JVM to stop processing early
3. **Escape Analysis**: Modern JVMs can eliminate intermediate allocations

### Example:

```java
List<Employee> employees = getEmployees();
double averageSalary = employees.stream()
    .filter(e -> e.getDepartment().equals("Engineering"))
    .mapToDouble(Employee::getSalary)
    .average()
    .orElse(0);
```

At execution time:

1. The JVM creates a pipeline but doesn't process any data yet
2. When `.average()` is called, the JVM analyzes the entire chain
3. It might optimize by:
   - Applying filter and map in a single pass
   - Avoiding creating intermediate Employee objects
   - Using primitive specializations to avoid boxing/unboxing

## JVM Optimizations for Functional Code

1. **Method Inlining**: Small methods, especially lambdas, are often inlined by the JIT compiler
2. **Specialized Implementations**: The JVM uses specialized implementations for common patterns
3. **Object Pooling**: For identical lambdas, the JVM may reuse the same instance

## Memory Considerations

1. **Capture Variables**:

   - When lambdas capture variables, the JVM must store them somewhere
   - For local variables, copies are created
   - This can lead to hidden object creation

2. **Primitive Streams**:
   - Special stream types (`IntStream`, `LongStream`, `DoubleStream`) avoid boxing/unboxing overhead
   - At the machine level, these work directly with primitive values

## Practical Applications

```java
// Example showing common patterns
List<Transaction> transactions = getTransactions();

// Sequential processing
double total = transactions.stream()
    .filter(t -> t.getYear() == 2023)
    .mapToDouble(Transaction::getAmount)
    .sum();

// Parallel processing
double parallelTotal = transactions.parallelStream()
    .filter(t -> t.getYear() == 2023)
    .mapToDouble(Transaction::getAmount)
    .sum();
```

In this example, the parallel version distributes work across multiple CPU cores. The JVM:

1. Divides the collection using spliterators
2. Assigns work to different threads via ForkJoinPool
3. Combines the results using specialized, thread-safe reduction operations

The Stream API and functional programming features represent a significant evolution in Java, providing higher-level abstractions while still leveraging the JVM's optimization capabilities to maintain performance.
