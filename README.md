# JSCoroutinesRecipes
Set of recipes for useful Kotlin/JS coroutines functions

## Async

Function used to create async coroutine:

```kotlin
fun launch(block: suspend () -> Unit) {
    block.startCoroutine(object : Continuation<Unit> {
        override val context = EmptyCoroutineContext
        override fun resume(value: Unit) = Unit
        override fun resumeWithException(exception: Throwable) = console.error(exception)
    })
}
```

Function used to create async coroutine that is returning `Promise`.

```kotlin
fun <T> async(x: suspend () -> T): Promise<T> = Promise { resolve, reject ->
    x.startCoroutine(object : Continuation<T> {
        override val context = EmptyCoroutineContext

        override fun resume(value: T) {
            resolve(value)
        }

        override fun resumeWithException(exception: Throwable) {
            reject(exception)
        }
    })
}
```

## Delay

Function used to delay coroutine excecution for some time. (Proposed by Andrey Mischenko)

```kotlin
suspend fun delay(time: Long): Unit = suspendCoroutine { continuation ->
    setTimeout({
        continuation.resume(Unit)
    }, time)
}

// We need top level setTimeout for node.js only. For browser usage you can use `kotlin.browser.window.setTimeout` from stdlib. But top level setTimeout works for browser as well
external fun setTimeout(function: () -> Unit, delay: Long)
```
