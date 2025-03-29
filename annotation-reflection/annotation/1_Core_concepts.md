# Core Concepts of Annotation in Java

## KTA

Imagine you're coloring a picture, and you want to leave notes about what colors to use. So you write little messages like "color this part blue" or "this is important" next to different parts of your drawing. In Java, annotations are like those little notes or sticky labels that programmers attach to their code. They don't change how the picture (or code) looks, but they give special instructions or information about it.

## Defination

Annotations in Java are a form of metadata that provide data about program elements (`classes`, `methods`, `variables`, `parameters`, etc.) without directly affecting the execution of the code. They serve as markers or instructions that can be read and processed by the compiler, development tools, or frameworks at compile-time or runtime through reflection.

Here are the essential concepts:

## 1. Basics of Annotations

- **Definition**: Annotations are tags that add metadata to Java code elements
- **Syntax**: Denoted by `@` symbol followed by the annotation name
- **Target Elements**: Can be applied to classes, methods, fields, parameters, and other program elements

## 2. Built-in Annotations

- **@Override**: Indicates a method is overriding a superclass method
- **@Deprecated**: Marks code as obsolete
- **@SuppressWarnings**: Tells compiler to suppress specific warnings
- **@FunctionalInterface**: Indicates an interface that can be used as a lambda expression

## 3. Meta-Annotations

- **@Retention**: Specifies how long the annotation should be kept:

  - `RetentionPolicy.SOURCE`: Discarded during compilation
  - `RetentionPolicy.CLASS`: Stored in class file but not available at runtime
  - `RetentionPolicy.RUNTIME`: Available at runtime through reflection

- **@Target**: Restricts which elements can be annotated:

  - `ElementType.TYPE`: Classes, interfaces
  - `ElementType.FIELD`: Fields
  - `ElementType.METHOD`: Methods
  - `ElementType.PARAMETER`: Method parameters
  - And others like CONSTRUCTOR, LOCAL_VARIABLE, etc.

- **@Documented**: Indicates that annotations should be included in Javadoc
- **@Inherited**: Allows annotations to be inherited by subclasses

## 4. Custom Annotations

- **Definition**: Created using `@interface` keyword
- **Elements**: Can contain elements that act like methods
- **Default Values**: Can specify default values for elements

## 5. Annotation Processing

- **Runtime Processing**: Using reflection API to access annotations at runtime
- **Compile-time Processing**: Using annotation processors to generate code or validate during compilation

## 6. Java 8+ Annotation Enhancements

- **Type Annotations**: Can annotate any use of a type (@NonNull, @Readonly)
- **Repeatable Annotations**: Can apply the same annotation multiple times to the same element

These concepts form the foundation of Java's annotation system, which is widely used in frameworks like Spring, Hibernate, JUnit, and many others for configuration, validation, and code generation.
