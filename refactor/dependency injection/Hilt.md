Why we need dependency injection :

Consider, that when we have to create a lot of objects which are dependent on many other objects in our project, it becomes tough when the project becomes bigger. With the code base increasing, we might need some good external support to manage it all. 
That is one of the use-cases for that we use a **dependency framework.**
Working on an Android project, we need to integrate a lot of different dependencies, and to manage these dependencies we use a dependency injection framework like Dagger.

What hilt for us :

- To make the dagger code easy and simple for developers.
- To just take care of where to inject dependencies and rest all of the code generations happens by dagger itself by using annotations and thus removing all the boilerplate code.
- To provide an easy way to provision different bindings to various build types, such as testing, debug, or release.

To understand Dagger we have to understand the 4 major annotations:

- Module
- Component
- Provides
- Inject

To understand it better in a basic way, think module as a provider of dependency and consider an activity or any other class as a consumer. Now to provide dependency from provider to consumer we have a bridge between them, in Dagger, Component work as that specific bridge.
Now, a module is a class and we annotate it with @Module for Dagger to understand it as Module. A component is an interface, which is annotated with @Component and takes modules in it.
**Provides** are annotation which is used in Module class to provide dependency and,
**Inject** is an annotation that is used to define a dependency inside the consumer.

**@HiltAndroidApp** :  If you are planning to use Dagger-Hilt in your app, the above mention step is a mandatory one. It generates all the component classes which we have to do manually while using Dagger.

**@Inject**is helping in passing the dependency required by ApiHelperImpl in the constructor itself.

```kotlin
class ApiHelperImpl @Inject constructor(private val apiService: ApiService) : ApiHelper {
    override suspend fun getUsers(): Response<List<User>> = apiService.getUsers()
}
```
**@InstallIn** annotation to install it in ApplicationComponent.
This means that the dependencies provided here will be used across the application. Let's consider that we want to use at the activity level we install the module in.
**@Singleton** annotation helps the instance to be created and used once across the app.
we also have @ActivityScoped, @FragmentScoped, etc in which dependencies are scoped till the lifecycle of Activity and Fragment.

**@AndroidEntryPoint** means Dagger-Hilt can now inject dependencies in this class.
**@AndroidEntryPoint** annotation can be used in,
1. Activity
2. Fragment
3. View
4. Service
5. BroadcastReceiver
**@ApplicationContext**
**@ActivityContext**

#### Qualifiers
Consider an example where we have two functions returning strings values. But while providing it via Dagger, how would dagger know which class needs which string value as they both are of the same type. A qualifier is an annotation that you use to identify a specific binding for a type when that type has multiple bindings defined.
or
In cases where you need Hilt to provide different implementations of the same type as dependencies, you must provide Hilt with multiple bindings. You can define multiple bindings for the same type with *qualifiers*.

Simple example of a Dagger 2 singleton component:
```kotlin
import dagger.Component;
import javax.inject.Singleton;

@Singleton
@Component(modules = { AppModule.class })
public interface AppComponent {

    void inject(MyApplication app);

    // Other injections
}
```

And an`AppModule`which uses`@Singleton`scoped provision methods might look like:
```kotlin
import dagger.Module;
import dagger.Provides;
import javax.inject.Singleton;

@Module
public class AppModule {
    @Singleton
    @Provides
    static MySingletonClass provideMySingletonClass()
    {
        return new MySingletonClass ();
    }
}
```

Question: when we use @Module(ApplicationComponenet) and if we don't use @Singelton in our provider function our dependecies is not singlton even we use @Module(ApplicationComponenet)  in our module?
answer:
componenet is a bridge between provider and consumer now `@InstallIn(SingletonComponent::class)` make your component is singlton not your dependecy or module.
```kotlin 
@Module
@InstallIn(ActivityComponent::class)   // this is for component
```

another question:
if our component is singleton what is feature ? 

There are three ways to define a binding in Hilt.
1. Annotate the constructor with `@Inject`
2. Use `@Binds` in a module
3. Use `@Provides` in a module

`@AndroidEntryPoint` generates an individual Hilt component for each Android class in your project. These components can receive dependencies from their respective parent classes as described in Component hierarchy.

**Component hierarchy**
Installing a module into a component allows its bindings to be accessed as a dependency of other bindings in that component or in any child component below it in the component hierarchy.
To obtain dependencies from a component, use the `@Inject` annotation to perform field injection:

```kotlin
@AndroidEntryPoint
class ExampleActivity : AppCompatActivity() {

  @Inject lateinit var analytics: AnalyticsAdapter
  ...
}
```
## Define Hilt bindings

To perform field injection, Hilt needs to know how to provide instances of the necessary dependencies from the corresponding component. A *binding* contains the information necessary to provide instances of a type as a dependency.

One way to provide binding information to Hilt is *constructor injection*. Use the `@Inject` annotation on the constructor of a class to tell Hilt how to provide instances of that class:
```kotlin
class AnalyticsAdapter @Inject constructor(
  private val service: AnalyticsService
) { ... }
```
### Inject interface instances with @Binds

Consider the `AnalyticsService` example. If `AnalyticsService` is an interface, then you cannot constructor-inject it. Instead, provide Hilt with the binding information by creating an abstract function annotated with `@Binds` inside a Hilt module.
The `@Binds` annotation tells Hilt which implementation to use when it needs to provide an instance of an interface.

- The function return type tells Hilt what interface the function provides instances of.
- The function parameter tells Hilt which implementation to provide.

```kotlin
interface AnalyticsService {
  fun analyticsMethods()
}

class AnalyticsServiceImpl @Inject constructor(
  ...
) : AnalyticsService { ... }

@Module
@InstallIn(ActivityComponent::class)
abstract class AnalyticsModule {

  @Binds
  abstract fun bindAnalyticsService(
    analyticsServiceImpl: AnalyticsServiceImpl
  ): AnalyticsService
}
```

Why we can not use @Provide for inject interfaces? answer I think @ provide needs body 
but in @Binds we need the implemenetion as parameter and return type is interface.
