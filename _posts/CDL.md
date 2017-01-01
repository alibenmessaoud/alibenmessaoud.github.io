# CountDownLatch—Friend or Foe?

# Alexey Soshin

Let’s imagine a situation in which we’d like to launch multiple concurrent tasks, and then wait on their completion before proceeding further. The `ExecutorService` makes the first part easy:

```
ExecutorService pool = Executors.newFixedThreadPool(8);
Future<?> future = pool.submit(() -> {
    // Your task here
});
```

But how do we wait for all of them to complete? `CountDownLatch` comes to our rescue. A `CountDownLatch` takes the number of invocations as a constructor argument. Each task then holds a reference to it, calling the `countDown` method when the task completes:

```
int tasks = 16;
CountDownLatch latch = new CountDownLatch(tasks);
for (int i = 0; i < tasks; i++) {
    Future<?> future = pool.submit(() -> {
        try {
            // Your task here
        }
        finally {
            latch.countDown();
        }
    });
}
if (!latch.await(2, TimeUnit.SECONDS)) {
    // Handle timeout
}
```

This example code will launch 16 tasks, then wait for them to finish before proceeding further. There are some important points to take note of, though:

1. Make sure that you release the latch in a `finally` block. Otherwise, if an exception occurs, your main thread may wait forever.
2. Use the `await` method that accepts a timeout period. That way, even if you forget about the first point, your thread will wake up sooner or later.
3. Check the return value of the method. It returns `false` if the time has elapsed, or `true` if all the tasks managed to complete on time.

As mentioned earlier, `CountDownLatch` receives its count on creation. It can be neither increased nor reset. If you’re looking for capabilities that are similar to those of `Count``DownLatch` but with the ability to reset the count, you should check out `CyclicBarrier` instead. 

`CountDownLatch` is useful in many different situations. It becomes especially useful when you’re testing your concurrent code, since it allows you to make sure that all the tasks are complete before checking their results.

Consider the following real-world example. You have a proxy and an embedded server, and you’d like to test that when the proxy is called, it invokes the correct endpoint on your server.

Obviously, it doesn’t make much sense to issue a request before both the proxy and server have started. One solution is to pass a `CountDownLatch` to both methods, and continue with the test only when both parties are ready:

```
CountDownLatch latch = new CountDownLatch(2);
Server server = startServer(latch);
Proxy proxy = startProxy(latch);
boolean timedOut = !latch.await(1, TimeUnit.SECONDS);
assertFalse(timedOut, "Timeout reached");
// Continue with test if assertion passes
```

You just need to make sure that both the `startServer` and `startProxy` methods call `latch.countDown` once they have successfully started.

`CountDownLatch` is very useful, but there’s one important catch: you shouldn’t use it in production code that makes use of concurrent libraries or frameworks, such as Kotlin’s coroutines, Vert.x, or Spring WebFlux. This is because `CountDownLatch` blocks the current thread. Different concurrency models don’t play well together.

- [Copy](https://learning.oreilly.com/library/view/97-things-every/9781491952689/ch15.html#)
- [Add Highlight](https://learning.oreilly.com/library/view/97-things-every/9781491952689/ch15.html#)
- [Add Note](https://learning.oreilly.com/library/view/97-things-every/9781491952689/ch15.html#)