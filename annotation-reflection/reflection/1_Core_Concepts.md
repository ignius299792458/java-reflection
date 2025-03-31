# Core Concepts of Reflection

Reflection in Java allows you to examine and modify the behavior of classes, interfaces, and methods at runtime. It's powerful but should be used carefully due to performance impacts and potential security concerns.

## 1. Basic Components

- **`java.lang.Class`**: The entry point to reflection
- **`java.lang.reflect` package**: Contains the main reflection APIs

## 2. Getting Class Objects

```java
// Method 1: Using .class syntax
Class<?> stringClass = String.class;

// Method 2: Using an object's getClass() method
String str = "Hello";
Class<?> strClass = str.getClass();

// Method 3: Using Class.forName() (for when you only have the name)
Class<?> dynamicClass = Class.forName("java.lang.String");
```

## 3. Examining Classes

```java
// Get all public methods (including inherited ones)
Method[] methods = myClass.getMethods();

// Get all declared methods (including private ones, but not inherited)
Method[] declaredMethods = myClass.getDeclaredMethods();

// Similarly for fields
Field[] fields = myClass.getFields();
Field[] declaredFields = myClass.getDeclaredFields();

// And for constructors
Constructor<?>[] constructors = myClass.getConstructors();
```

## 4. Accessing and Modifying Fields

```java
// Get a specific field
Field nameField = Person.class.getDeclaredField("name");

// Make private field accessible
nameField.setAccessible(true);

// Get and set field values
Person person = new Person();
String name = (String) nameField.get(person);
nameField.set(person, "New Name");
```

## 5. Invoking Methods

```java
// Get a specific method (with parameter types)
Method sayHelloMethod = Person.class.getMethod("sayHello", String.class);

// Invoke the method
Person person = new Person();
sayHelloMethod.invoke(person, "World");  // Equivalent to person.sayHello("World")

// For static methods, the first parameter can be null
Method staticMethod = Math.class.getMethod("abs", int.class);
int result = (int) staticMethod.invoke(null, -5);  // Equivalent to Math.abs(-5)
```

## 6. Creating Objects

```java
// Get constructor
Constructor<Person> constructor = Person.class.getConstructor(String.class, int.class);

// Create new instance
Person person = constructor.newInstance("John", 30);

// Alternative for no-arg constructor
Person defaultPerson = Person.class.newInstance();  // Deprecated in newer Java versions
```

## 7. Working with Arrays

```java
// Create array using reflection
int[] intArray = (int[]) Array.newInstance(int.class, 5);

// Get and set array elements
Array.set(intArray, 0, 42);
int value = (int) Array.get(intArray, 0);
```

## 8. Handling Generics

```java
// Get generic type information
Field listField = MyClass.class.getDeclaredField("myList");
Type genericType = listField.getGenericType();

if (genericType instanceof ParameterizedType) {
    ParameterizedType pType = (ParameterizedType) genericType;
    Type[] typeArguments = pType.getActualTypeArguments();
    // typeArguments[0] would be String.class for List<String>
}
```

## 9. Working with Annotations

```java
// Check if an annotation is present
boolean hasAnnotation = MyClass.class.isAnnotationPresent(MyAnnotation.class);

// Get the annotation
MyAnnotation annotation = MyClass.class.getAnnotation(MyAnnotation.class);

// Read annotation values
String value = annotation.value();
```

## 10. Dynamic Proxies

```java
// Create a dynamic proxy implementing an interface
MyInterface proxy = (MyInterface) Proxy.newProxyInstance(
    MyInterface.class.getClassLoader(),
    new Class<?>[] { MyInterface.class },
    new InvocationHandler() {
        @Override
        public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
            // Custom behavior before method call
            System.out.println("Before calling: " + method.getName());
            
            // Actual method call or custom implementation
            return "Result from " + method.getName();
        }
    }
);
```

Reflection is powerful for framework development, but remember it has downsides including performance overhead, loss of compile-time checking, and security restrictions in certain environments.