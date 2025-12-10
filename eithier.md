# Understanding Either in Dart 🟢

`Either` is a generic type in Dart used for handling **success or failure** in a structured way.  
It is especially useful for asynchronous operations and avoids exceptions or nulls.

---

## 1. What Is Either?

`Either` can hold **one of two possible values**:

- **Left** → usually represents a **failure** or error  
- **Right** → usually represents a **success** or valid data  

> Only one of them will be stored at a time.

**In simple terms:**

- **Go** returns multiple values: `(data, error)`  
- **Dart** returns a single value: `Either`, which contains **success or failure**.

---

## 2. Why Use Either?

Dart does not support multiple return values like Go. `Either` provides a clean way to return either:

- an **error**, or  
- a **result**

**Benefits of using Either:**

- Avoids throwing exceptions everywhere  
- Cleaner function signatures  
- Easier to test  
- Easier to handle failures  
- Works well with asynchronous functions

---

## 3. How Either Works Internally

When a function returns:

```dart
Future<Either<Failure, User>>
The following happens:

The function runs asynchronously because of Future.

After the async work completes:

On success → returns Right(User)

On failure → returns Left(Failure)

The calling code reads the value using:

fold()

or isLeft / isRight

This makes error handling predictable and structured.

4. Why Left and Right?
Either is defined as:

dart
Copy code
Either<L, R>
Where:

L → type for the Left value (error)

R → type for the Right value (success)

Example:

dart
Copy code
class Failure {
  final String message;
  Failure(this.message);
}

class User {
  final String name;
  User(this.name);
}
5. Practical Example
Function returning Either
dart
Copy code
Future<Either<Failure, User>> fetchUser(String id) async {
  if (id.isEmpty) {
    return Left(Failure("ID cannot be empty"));
  }

  try {
    final user = await db.get(id);
    return Right(user);
  } catch (e) {
    return Left(Failure(e.toString()));
  }
}
Using the result
dart
Copy code
final result = await fetchUser("123");

result.fold(
  (failure) => print("Error: ${failure.message}"),
  (user)     => print("User: ${user.name}"),
);
6. Why Choose Either Over Other Methods?
Method	Problem	Why Either is Better
Throwing exceptions	Expensive, breaks control flow	Avoids exceptions
Returning null	Does not provide error information	Carries complete error details
Returning object + bool	Not type-safe, messy	Clean, structured
Go-style (value, error)	Dart does not support multiple return values	Either provides similar behavior

7. Comparison: Go vs Dart
Go example:

go
Copy code
user, err := fetchUser(id)
if err != nil {
    return err
}
Dart example using Either:

dart
Copy code
final result = await fetchUser(id);

result.fold(
  (err) => print(err.message),
  (user) => print(user.name),
);
Summary:

Go → (value, error)

Dart → Either<Error, Value>

Both serve the same purpose.

8. Understanding Either as a Generic Class
Either<Failure, User> means:

Left side → Failure

Right side → User

Examples of flexibility:

Either<Failure, String>

Either<Failure, int>

Either<Failure, List<User>>

Makes it highly reusable.

9. Useful Either Methods
9.1 fold() — The Big Boss
Think of fold like a switch-case for Either.

Go:

go
Copy code
user, err := fetchUser()
if err != nil {
    // Left
} else {
    // Right
}
Dart:

dart
Copy code
result.fold(
  (failure) => print("Error"),
  (user) => print("Success"),
);
Why fold is awesome:

Forces handling both error (Left) and success (Right)

No skipping error handling

No nulls

9.2 isRight() / isLeft()
Simple type checks

Useful when you care about only success or error, not both

dart
Copy code
if (result.isRight()) {
  final user = result.getOrElse(() => DefaultUser());
  print(user.name);
}
9.3 getOrElse()
Extracts the success value or returns a fallback:

dart
Copy code
final user = result.getOrElse(() => User("Guest"));
9.4 map() — Transform Right (Success)
Applies transformation only when Right:

dart
Copy code
final greeting = result.map((user) => "Hello ${user.name}");
If Left → does nothing

Shorter and cleaner than using fold

9.5 mapLeft() — Transform Left (Error)
Like Go’s fmt.Errorf:

dart
Copy code
result.mapLeft(
  (failure) => ValidationFailure("Better error: ${failure.message}")
);
9.6 flatMap() — VERY IMPORTANT
Used when your next function also returns Either:

dart
Copy code
Future<Either<Failure, User>> fetchUser();
Future<Either<Failure, Profile>> fetchProfile(String userId);

final result = await fetchUser("1")
    .flatMap((user) => fetchProfile(user.id));
Avoids nested fold hell

Go equivalent:

go
Copy code
user, err := fetchUser()
if err != nil { return nil, err }

profile, err := fetchProfile(user.ID)
if err != nil { return nil, err }
10. Summary
Either wraps success or error in a single object

Left → error

Right → success

Uses generics for flexible types

Works perfectly with async operations

Provides a clean, structured error-handling approach

Similar to Go’s (value, error) pattern but implemented using Dart

yaml
Copy code

---

✅ This is now a **full Markdown version** ready for GitHub — with:

- Proper headings  
- Code blocks  
- Tables  
- Bullet points  
- All your original content preserved  

---

If you want, I can also **add collapsible sections and emojis** to make it more **beginner-friendly and visually appealing** for GitHub README.  

Do you want me to do that?