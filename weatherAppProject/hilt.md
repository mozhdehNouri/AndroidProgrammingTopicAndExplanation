Dependency injection with Hilt

Why we need dependency injection :

Consider, that when we have to create a lot of objects which are dependent on many other objects in
our project, it becomes tough when the project becomes bigger. With the code base increasing, we
might need some good external support to manage it all. That is one of the use-cases for that we use
a**dependency framework.**

Working on an Android project, we need to integrate a lot of different dependencies, and to manage
these dependencies we use a dependency injection framework like Dagger.

What hilt for us :

- To make the dagger code easy and simple for developers.
- To provide a different set of bindings for different build types.
- To just take care of where to inject dependencies and rest all of the code generations happens by
  dagger itself by using annotations and thus removing all the boilerplate code.

Basically, to understand Dagger we have to understand the 4 major annotations,

- Module
- Component
- Provides
- Inject

To understand it better in a basic way, think module as a provider of dependency and consider an
activity or any other class as a consumer. Now to provide dependency from provider to consumer we
have a bridge between them, in Dagger, Component work as that specific bridge.

Now, a module is a class and we annotate it with @Module for Dagger to understand it as Module. A
component is an interface, which is annotated with @Component and takes modules in it.

Provides are annotation which is used in Module class to provide dependency and,

Inject is an annotation that is used to define a dependency inside the consumer.

**@HiltAndroidApp** :  If you are planning to use Dagger-Hilt in your app, the above mention step is
a mandatory one. It generates all the component classes which we have to do manually while using
Dagger.

Here,**@Inject**is helping in passing the dependency required by ApiHelperImpl in the constructor
itself.

```kt
class ApiHelperImpl @Inject constructor(private val apiService: ApiService) : ApiHelper {

    override suspend fun getUsers(): Response<List<User>> = apiService.getUsers()

}
```

```java

@Module
@InstallIn(ApplicationComponent::class)
class ApplicationModule {
}
```

Here, you can see that we have used**@InstallIn**annotation to install it in ApplicationComponent.
ApplicationComponent is provided by Dagger-Hilt.

This means that the dependencies provided here will be used across the application. Let's consider
that we want to use at the activity level we install the module in.

Here, we have provided dependencies using**@Provide**annotation, which would be accessed across the
application.

**@Singleton**annotation helps the instance to be created and used once across the app.

Similarly, like Singleton which stays till the application lifecycle, we also have @ActivityScoped,
@FragmentScoped, etc in which dependencies are scoped till the lifecycle of Activity and Fragment.

Here,**@AndroidEntryPoint**means Dagger-Hilt can now inject dependencies in this class.

**@AndroidEntryPoint**annotation can be used in,

1. Activity
2. Fragment
3. View
4. Service
5. BroadcastReceiver

**@ApplicationContext**

**@ActivityContext**

#### Qualifiers

Consider an example where we have two functions returning strings values. But while providing it via
Dagger, how would dagger know which class needs which string value as they both are of the same
type.

A qualifier is an annotation that you use to identify a specific binding for a type when that type
has multiple bindings defined

question is :

use @InstallIn(SingletonComponent::class) in my module class
and i provide some dependency with @Provide and @singlton
my question is when my component is @Singleton why i need to use @Singleton in my function
thay can not provide @singleton when i use singleton component ?

answer :

Using`@InstallIn(SingletonComponent::class)`in your Dagger-Hilt module tells Hilt to install that
module in the`SingletonComponent`. This essentially means that the lifespan of the dependencies
provided by this module will be the same as that of the`SingletonComponent`, which typically lasts
for the entire lifetime of the application.

When you use`@Singleton`on your provider functions, you’re telling Hilt, more specifically, that the
instance provided by that function should be a singleton instance. So, even if the module is
installed in the`SingletonComponent`, if you don’t annotate a`@Provides`function with`@Singleton`,
Hilt won’t treat the provided instance as a singleton.

Here’s why this distinction is necessary:

1. Scoping: The`@Singleton`annotation is a scope annotation that indicates that a single instance
   should be created and shared. It’s a promise from you to the DI framework that this class can be
   safely shared. The container (in this case, the`SingletonComponent`) respects this guarantee and
   will not create more than one instance.

2. Flexibility: Within the same component, you might want some instances to be shared (singleton)
   and others to be created every time they are injected. This flexibility allows for better memory
   management and control over the instances.

Now to the second part of your question, here’s a simple example of a Dagger 2 singleton component:

```kts
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

```kts
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

In this case,`AppComponent`is a singleton component in Dagger 2, and`MySingletonClass`is provided as
a singleton instance by the`AppModule`. Every time`MySingletonClass`is required, the same instance
will be provided until the application is alive.
