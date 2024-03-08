Everything about the networking include retrofit, okhttp, diffrent type of json parser

What is json and why we need it and how it is work internally :

JSON, which stands for JavaScript Object Notation, is a lightweight data interchange format. It is
easy for humans to read and write, and easy for machines to parse and generate. JSON is a text
format that is completely language-independent but uses conventions that are familiar to programmers
of the C family of languages, including C, C++, C#, Kotlin, Java, JavaScript, Python, and many
others.

JSON data is represented as key-value pairs, where keys are strings and values can be strings,
numbers, booleans, arrays, objects, or null.

We will use the following JSON string to convert to the Kotlin (data) Class.

**How a JSON parser works:**

1. **Tokenization:**The parser reads the JSON text and breaks it down into tokens. Tokens are the
   smallest units like keys, values, braces, brackets, commas, etc.

2. **Syntax Analysis:**After tokenizing, it analyzes the tokens based on JSON syntax rules. For
   example, it knows that a colon should follow a key and a comma separates key-value pairs.

3. **Data Structure Creation:**It then constructs data structures corresponding to the JSON
   document’s array and object notation, mapping it to arrays, lists, or objects in the programming
   language.

4. **Object Mapping:**In the case of automatic object mapping (like when using Gson or Jackson
   libraries in Android), the parser further interprets these data structures to create instances of
   classes with the JSON data mapped to their fields.

5. **Error Handling:**If there are issues in the JSON structure or if there are mismatches in types
   or structures during object mapping, the parser will handle errors according to its
   configuration. It might throw exceptions or fail silently.

when use each one of them?
Each JSON converter library in Android offers different features and has its own pros and cons, and
the choice often depends on the specific needs of your project, as well as personal preference.
Below, I’ll provide a brief overview of each one and scenarios they are typically used for:

1. **Gson:**

    - *Usability:*Gson is one of the easiest libraries to start with due to its straightforward API.
    - *Popularity:*Since it’s been around for quite some time, it has a large user base and
      extensive documentation.
    - *Use When:*Your JSON parsing needs are straightforward or you are working on a legacy Android
      project that used Gson.
    - *Limitation:*Slower than some modern libraries and does not support Kotlin features natively.

2. **Moshi:**

    - *Usability:*Moshi is similar to Gson but is newer and often considered its “spiritual
      successor”. It supports modern features like Kotlin’s nullable types and default values.
    - *Performance:*Generally faster than Gson and handles various edge cases more gracefully.
    - *Use When:*Your project uses Kotlin and you need a balance between ease of use and
      performance.

3. **Kotshi:**

    - *Functionality:*An annotation processor built on top of Moshi. It aims to make JSON
      serialization and deserialization in Kotlin simple and fast.
    - *Performance:*Generates adapter classes at compile-time, which means faster performance at
      runtime.
    - *Use When:*Performance is a priority, and you are already using Moshi in a Kotlin project.

4. **kotlinx.serialization:**

    - *Integration:*It’s part of the Kotlin ecosystem, developed by JetBrains, and therefore has
      excellent support for Kotlin features like coroutines, flows, and serialization of Kotlin
      classes.
    - *Performance:*Because it’s built into the Kotlin compiler as a plugin, it can optimize
      bytecode for serialization and deserialization tasks.
    - *Use When:*You are exclusively using Kotlin and want tight integration with Kotlin features
      and future-proofing for any JVM-targeted development.

Each of these libraries requires a bit of setup and has its own annotation-based or reflective
process to handle serialization and deserialization. They may also provide customizations, such as
handling date formats, ignoring unknown properties, or custom field naming policies.

**Why use different ones:**

- **Legacy Code Support:**You might inherit a project already using Gson and continue using it for
  compatibility.
- **Kotlin Support:**If your codebase is in Kotlin and you want optimal performance and
  compatibility, kotlinx.serialization or Moshi may be preferable.
- **Performance Needs:**If you need better performance during parsing (like for large JSON datasets
  or resource-constrained devices), you might favor a library that generates code during
  compile-time, such as Kotshi.
- **Library Size:**If you’re aiming for a smaller APK size, consider the overhead each library adds
  to your app.

Remember that while JSON parsing is a common task in many Android applications, it’s not the only
consideration when choosing a library. You should also take into account factors like the size of
your project, your team’s familiarity with the library, and the support and maintenance prospects of
the library.

what is serilaze and deserilaze :
Serialization and deserialization are fundamental concepts in computer science related to the
process of converting data structures or object states into a format that can be stored,
transferred, and reconstructed later.

**Serialization**is the process of transforming an object or data structure into a format that can
be easily saved to storage or transmitted across a network. The format could be JSON, XML, binary,
or any other form that can reliably represent the object’s state. This allows data to be
persistently stored, or sent to another system or application, irrespective of the platform or
programming language.

In the context of JSON serialization in Android or Kotlin/Java:

- **Serialization**would be converting a Kotlin or Java object into a JSON string. This is useful
  when you need to send data from an Android app to a server or save it in a readable and structured
  text format.

Example in Kotlin with`kotlinx.serialization`:

```kt
import kotlinx.serialization.*
import kotlinx.serialization.json.*

@Serializable
data class User(val name: String, val age: Int)

fun main() {
    val user = User("Alice", 30)
    val jsonString = Json.encodeToString(user)
    println(jsonString)  // Output: {"name":"Alice","age":30}
}
```

**Deserialization**is the reverse process, where you take the serialized string or file, and
reconstruct the original object or data structure. This is commonly used when receiving data from a
server or loading a stored file, enabling you to work with the received data within your application
using native data types.

In the context of JSON deserialization in Android or Kotlin/Java:

- **Deserialization**would be converting a JSON string back into a Kotlin or Java object. This is
  essential when you receive JSON data from a web API and want to work with it in your app.

Example in Kotlin with`kotlinx.serialization`:

```kt
import kotlinx.serialization.*
import kotlinx.serialization.json.*

@Serializable
data class User(val name: String, val age: Int)

fun main() {
    val jsonString = """{"name":"Alice","age":30}"""
    val user = Json.decodeFromString<User>(jsonString)
    println(user.name)  // Output: Alice
}
```

Both serialization and deserialization are crucial for data exchange and persistent storage, as they
allow complex objects to be easily shared or saved without loss of structure or information.



----

why not Gson :

> A Java serialization/deserialization library to convert Java Objects into JSON and back

Which obviously is not Kotlin-Friendly or written in Kotlin, So one can clearly understand that this
is not a Modern library anymore. It’s been out there since 2008, So it’s been 12 years, and it seems
not being updated anymore.

Method count wise, Gson has ~1036 methods which are almost twice as Moshi (~500)!

❌ Footprint wise, Gson adds ~300kB to your APK size compared to ~120kB!

❌Gson only can use reflection to serialize/deserialize JSON strings. Compared to Moshi’s Kotlin
code-gen support.

❌Gson does not support default values for fields, If a field is missing in the response JSON string
and you’ve set its default value to something rather than null, It will be null afterwards!

why moshi is good ?

Kotlin friendly, fast, reliable with lots of features. Let’s review some of Moshi’s features:

- Moshi has a newBuilder() API which can be used to create new adapters from upstream adapters,
  Something similar to OkHttp or Okio builders, This is super helpful because you can create
  separate adapters so you don’t end up with having a god adapter which knows how to parse 1K+
  models.
- Moshi has built-in support for polymorphic datatypes which also has fallback support for unknown
  types!
- Moshi has Code-gen adapter for Kotlin, This is great! With help of annotations, you make the
  Serialization/Deserialization much faster and bypass the old slow reflection method that Gson
  uses!

Moshi is way faster than
Gson([Link1](https://zacsweers.github.io/json-serialization-benchmarking/), ) and uses less memory,
This is due to its usage of Okio which can predict or expect ahead of the time the keys which helps
on ignoring the unknown or unwanted fields while parsing a
stream ([A good article on this](https://medium.com/@BladeCoder/advanced-json-parsing-techniques-using-moshi-and-kotlin-daf56a7b963d)).
Retrofit is also using Okio under the hood, So guess what happens? The JSON serialization library(
Moshi) and networking library(Retrofit) are
sharing[the buffer their using](https://github.com/square/retrofit/blob/master/retrofit-converters/moshi/src/main/java/retrofit2/converter/moshi/MoshiResponseBodyConverter.java#L35-L45)
which leads to much lower memory consumption while making network calls and serializing the
response!

Add[@JsonClass](http://twitter.com/JsonClass)(generateAdapter = true) annotation to all data classes
Which are going to participate in the serializing/deserializing JSON process. This helps Moshi to
use code-gen and not to use reflection which will increase the speed of the process:
