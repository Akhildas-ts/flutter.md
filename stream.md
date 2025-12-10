# Dart Streams — A Practical Guide for Go Developers 🟢

Streams in Dart are similar to **channels in Go**, allowing you to handle **multiple asynchronous values** over time.

---

## 1. Go Channels vs Dart Streams

| Concept                | Go Channel                       | Dart Stream                               |
|------------------------|---------------------------------|------------------------------------------|
| Sends many values       | ✅ Yes                          | ✅ Yes                                   |
| Receiving values        | `range` over channel            | `await for` or `.listen()`              |
| Producer → send         | `ch <- value`                   | `yield` (inside `async*` function)      |
| Consumer → receive      | `<-ch`                           | `.listen()` or `await for`               |
| Close channel/stream    | `close(ch)`                     | Closes automatically when done          |

> Dart streams can be **single or multiple values**:  
> - `async*` → multiple values (like Go channels)  
> - `async` → single value (like Future)

---

## 2. Creating a Simple Stream

```dart
Stream<int> countStream() async* {
  for (int i = 0; i < 5; i++) {
    yield i; // sends value to the consumer
  }
}
Explanation:

async* marks the function as a generator that produces multiple values.

yield sends a value to the stream’s consumer.

No manual closing is needed — stream closes automatically after all values are sent.

3. Consuming a Stream
Using await for
dart
Copy code
void main() async {
  await for (var i in countStream()) {
    print(i);
  }
}
Explanation:

await for loops asynchronously over the stream.

Each value is received one by one as it is emitted.

Similar to ranging over a Go channel.

Using .listen() (Alternative)
dart
Copy code
void main() {
  countStream().listen((i) {
    print(i);
  });
}
Explanation:

.listen() registers a callback to run each time a value is emitted.

Non-blocking: execution continues immediately.

Useful when you don’t want to pause the main function with await.

4. Summary
Dart streams are the equivalent of Go channels for sending multiple values asynchronously.

async* functions produce multiple values with yield.

Consumers can use await for (blocking-like) or .listen() (non-blocking).

Streams close automatically after all values are sent.

Use async functions for single future values and async* functions for multiple streaming values.