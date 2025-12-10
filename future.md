# Dart Async vs Go Concurrency — A Practical Guide for Go Developers 🟢

This guide explains **Dart asynchronous programming** and compares it to **Go concurrency** concepts for developers coming from Go.

---

## 1. Go: Goroutines

```go
go func() {
    result := fetchData()
    fmt.Println(result)
}()


Explanation:

go keyword runs the function concurrently.

Execution continues without waiting.

The goroutine cannot directly return a value to the caller.

Output may appear later depending on scheduling.

2. Go: Goroutines with Channels
ch := make(chan string)

go func() {
    data := fetchData()
    ch <- data
}()

result := <-ch


Explanation:

Channels allow goroutines to send data back to the caller.

ch <- data sends a value to the channel.

<-ch blocks until a value is received.

This is Go’s standard pattern for waiting for asynchronous results.

3. Dart: Future (Go Equivalent of "Value Coming Later")
Future<String> fetchData() async {
  await Future.delayed(Duration(seconds: 1));
  return "Data";
}


Explanation:

Future<T> represents work happening in the background.

async marks the function as asynchronous.

await pauses execution inside the function until the operation completes.

Returning a value from an async function wraps it in a Future.

4. Dart: Await (Equivalent to Blocking Receive <-ch)
void main() async {
  String result = await fetchData();
  print(result);
}


Explanation:

await waits until the Future is completed.

Similar to blocking on a channel: result := <-ch.

main() must be marked async if it uses await.

5. Dart: Then() (Callback Style, Non-Blocking)
fetchData().then((result) {
  print(result);
});


Explanation:

.then() schedules a callback when the Future completes.

Non-blocking: execution continues immediately.

Comparable to running a goroutine and later receiving a result through a callback-like pattern.

No need to manually create channels.

6. When the Function Returns Nothing (Future<void>)
Future<void> fetchUser() async {
  print("Done");
}

// Using .then()
fetchUser().then((value) {
  print(value); // value is null
});


Explanation:

.then() still runs even if the function returns nothing.

value will be null.

You can name it anything: value, _, ignored.

Example:

fetchUser().then((_) {
  print("finished");
});


Behaves like sending an empty signal on a Go channel:

done := make(chan struct{})
done <- struct{}{}

7. Dart Error Handling
Method 1: try-catch (similar to if err != nil)
Future<void> loadUser() async {
  try {
    final user = await fetchUser();
    print(user.name);
  } catch (e) {
    print("Error: $e");
  }
}

Method 2: catchError() (callback style)
fetchUser()
  .then((user) => print(user.name))
  .catchError((error) => print("Error: $error"));


Explanation:

catchError works like a callback-based error handler.

Similar to Go running a goroutine and checking error inside it.

8. Blocking vs Non-Blocking (Key Difference)
Blocking (await)
final data = await fetchData();


Pauses execution inside the current async function.

Similar to <-ch in Go.

Non-Blocking (.then())
fetchData().then((data) {
  print(data);
});


Does not wait.

Program continues immediately.

Think of it as launching a goroutine and letting it handle the result asynchronously.

9. Summary Table
Concept	Go	Dart
Background task	go func()	Future()
Wait for result	<-ch	await
Non-blocking callback	goroutine + callback logic	.then()
Error handling	if err != nil	try-catch, .catchError()
No return value	empty struct{}	Future<void> + _

✅ This guide makes it clear for Go developers to understand Dart async patterns with minimal friction.


---

This is **ready to save as `Dart-Async-vs-Go.md`**.  

It includes:

- Sharp, beginner-friendly points  
- Proper headings  
- Clean code blocks  
- Go vs Dart comparisons  
- A summary table  

---

If you want, I can **combine this with the Either guide** into a **full “Dart for Go Developers” GitHub Markdown series**, with proper navigation and links between sections.  

Do you want me to do that?