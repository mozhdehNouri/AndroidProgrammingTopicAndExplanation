
For finding performance issue :

- Benchmarking
- General performance metrics


**Benchmarking:**
It's difficult to compare the performance of algorithms simply by looking at their descriptions or pseudocode. It's better to run actual implementations on a real system. This method of measurement is called benchmarking. There are three categories of benchmark:

- Microbenchmarks: These are metrics showing the performance of certain functions. They assume that a small piece of business logic is contained in a single function, and we simply measure how fast this function runs. 

- Macrobenchmarks: These are the opposite of microbenchmarks; they test the entire application.

- Mesobenchmarks: These are something in-between, measuring features or workflows.

**Microbenchmarks:**

Microbenchmarking is used if we want to know which methods perform better than others. A naive approach would wrap the called function inside a block of a measureTimeMillis or measureNanoTime function. These functions just calculate the difference between start and stop timestamps:
```kotlin
public inline fun measureTimeMillis(block: () -> Unit) : Long { val start = System.currentTimeMillis() block() return System.currentTimeMillis() - start }
```

