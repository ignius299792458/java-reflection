# Custom Annotations in Java

Annotations in Java are a way to add metadata to your code. They don't directly affect how your code runs but provide information to the compiler, runtime environment, or other tools.

## Basic Concept

Think of annotations as labels or tags you stick on your code. These labels can be read by Java tools to do special things.

## Creating a Custom Annotation

```java
// Step 1: Define your annotation
@Retention(RetentionPolicy.RUNTIME) // When is it available?
@Target(ElementType.METHOD)         // Where can it be used?
public @interface MyAnnotation {    // @interface, not interface!
    String value() default "Default value"; // Element with default value
    int count() default 0;                  // Another element
}
```

## Key Components

1. **`@interface`**: This tells Java you're making an annotation, not a regular interface.

2. **Meta-annotations** (annotations for your annotation):
   - `@Retention`: How long the annotation sticks around
     - `SOURCE`: Compiler discards it
     - `CLASS`: Kept in .class file but not at runtime
     - `RUNTIME`: Available at runtime through reflection
   
   - `@Target`: Where the annotation can be placed
     - `METHOD`: Only on methods
     - `FIELD`: Only on fields
     - `TYPE`: On classes, interfaces
     - `PARAMETER`: On method parameters
     - Multiple targets: `@Target({ElementType.METHOD, ElementType.FIELD})`

3. **Elements**: The data your annotation carries (like methods in an interface)

## Using Your Annotation

```java
public class MyClass {
    @MyAnnotation(value = "Test method", count = 3)
    public void testMethod() {
        // Method code
    }
    
    @MyAnnotation // Using default values
    public void anotherMethod() {
        // Method code
    }
}
```

## Processing Annotations

To make annotations useful, you need to process them:

```java
public class AnnotationProcessor {
    public static void processAnnotations(Class<?> clazz) {
        // Get all methods from the class
        for (Method method : clazz.getDeclaredMethods()) {
            // Check if our annotation is present
            if (method.isAnnotationPresent(MyAnnotation.class)) {
                // Get the annotation
                MyAnnotation annotation = method.getAnnotation(MyAnnotation.class);
                
                // Use the annotation's elements
                System.out.println("Method: " + method.getName());
                System.out.println("Annotation value: " + annotation.value());
                System.out.println("Count: " + annotation.count());
            }
        }
    }
    
    public static void main(String[] args) {
        processAnnotations(MyClass.class);
    }
}
```

## Common Use Cases

1. **Documentation**: Like `@Deprecated` to mark outdated methods
2. **Compile-time checks**: Tools like Lombok use annotations to generate code
3. **Runtime processing**: Frameworks like Spring use annotations to wire components
4. **Testing**: JUnit uses annotations like `@Test` to identify test methods

## Advanced Features

1. **Repeatable annotations**: Same annotation multiple times on one element
2. **Inheritance**: Whether annotations are passed to subclasses
3. **Type annotations**: Annotations on any use of a type (Java 8+)
