

Interceptors:
Interceptors, are a powerful mechanism that can monitor, rewrite, and retry the API call.
Interceptors function similarly to airport security personnel during the security check process. They checked our boarding pass, stamped it, and then let us pass.
We can use interceptors to do a variety of things, such as centrally monitoring API calls.

**Interceptor Types**
- **Application Interceptors**: Interceptors added between the Application Code (our written code) and the OkHttp Core Library are referred to as application interceptors. These are the interceptors that we add with addInterceptor().
- **Network Interceptors**: Interceptors on the network: These are interceptors placed between the OkHttp Core Library and the server. These can be added to OkHttpClient by using the addNetworkInterceptor command ().

```kotlin 
fun myHttpClient(): OkHttpClient {
     val builder = OkHttpClient().newBuilder()
         .addInterceptor(/*our interceptor*/)
     return builder.build()
 }
```
**Create Interceptor:**
```kotlin
class MyInterceptor : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        /**
         * Our API Call will be intercepted here
         */
    }
}
```

useCase of interceptor:

- Access response code:
```kotlin
class ErrorInterceptor  : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val aRequest: Request = chain.request()
        val aResponse = chain.proceed(request)
        when (aResponse.code()) {
            400 -> {
                // Show Bad Request Error Message
            }
            401 -> {
                // Show UnauthorizedError Message
            }

            403 -> {
                // Show Forbidden Message
            }

            404 -> {
                // Show NotFound Message
            }

            // ... and so on
        }
        return response
    }
}
```

