# Dart vs Go: Variables, Types, Null Safety, Functions, and OOP

## 1. Variables & Type

### **Go**

```go
// Explicit type
var name string = "Akhil"

// Type inference
age := 20
```

### **Dart**

```dart
// Explicit type
String name = "Akhil";

// Type inference
var age = 20;
```

**Notes:**

* Go uses `var` for explicit type declaration and `:=` for type inference.
* Dart uses `var` for type inference; type can also be explicitly declared.

---

## 2. Null Safety

### **Go**

```go
var name *string  // pointer can be nil
var age int       // cannot be nil, default zero value is 0

if name != nil {
    fmt.Println(*name)
}
```

### **Dart**

```dart
String? name;   // nullable variable (can be null)
int age = 0;    // non-nullable

if (name != null) {
  print(name);  // Smart cast!
}
```

**Notes:**

* Go uses pointers (`*type`) for nullable variables.
* Dart uses `?` to mark a variable as nullable.
* Non-nullable types in both languages always have a default value (Go: zero value, Dart: must be initialized).

---

## 3. Functions

### **Go**

```go
func add(a int, b int) int {
    return a + b
}

result := add(2, 3)
```

### **Dart**

```dart
int add(int a, int b) {
  return a + b;
}

var result = add(2, 3);
```

**Notes:**

* Both languages have strong typing in function parameters and return types.
* Dart supports optional and named parameters, Go does not.

---

## 4. Classes & OOP

### **Go** (Structs + Methods)

```go
type Person struct {
    Name string
    Age  int
}

func (p Person) Greet() {
    fmt.Println("Hello, my name is", p.Name)
}

p := Person{Name: "Akhil", Age: 20}
p.Greet()
```

### **Dart**

```dart
class Person {
  String name;
  int age;

  Person(this.name, this.age);

  void greet() {
    print("Hello, my name is $name");
  }
}

var p = Person("Akhil", 20);
p.greet();
```

**Notes:**

* Go uses `structs` and attaches methods for OOP behavior.
* Dart has full OOP support with `class`, constructors, methods, and inheritance.

---

## ✅ Key Takeaways

| Feature              | Go                                 | Dart / Flutter                                 |
| -------------------- | ---------------------------------- | ---------------------------------------------- |
| Variable Declaration | `var` (explicit) / `:=` (inferred) | `var` (inferred) / type explicit               |
| Null Safety          | Pointers `*type` for nullable      | `?` for nullable types                         |
| Functions            | Strongly typed, no default params  | Strongly typed, supports optional/named params |
| OOP                  | Struct + methods, no inheritance   | Full classes, constructors, inheritance        |
| Default Values       | Zero value (0, "", false, nil)     | Must initialize non-nullable variables         |
