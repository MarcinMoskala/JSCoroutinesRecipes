# JSCoroutinesRecipes
Set of recipes for useful Kotlin/JS coroutines functions

## Async

Function used to create async coroutine context that is returning `Promise`.

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
        println("timeout")
        continuation.resume(Unit)
    }, time)
}

external fun setTimeout(function: () -> Unit, delay: Long)
```
