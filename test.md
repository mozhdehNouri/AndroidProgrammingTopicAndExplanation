### Test

- `main`: Contains your app code.
- `androidTest`: Contains tests known as instrumented tests.
- `test`: Contains tests known as local tests.



###### Local tests (test source set)

These tests are run locally on your development machine’s JVM and do not require an emulator or physical device. Because of this, they run fast, but their fidelity is lower, meaning they act less like they would in the real world.

###### Instrumented tests (androidTest source set)

These tests run on real or emulated Android devices, so they reflect what will happen in the real world, but are also much slower.

###### **Test runner**

A test runner is a JUnit component that runs tests. Without a test runner, our tests would not run. There’s a default test runner provided by JUnit that we get automatically.

```kt
android {

    defaultConfig {
        testInstrumentationRunner 'androidx.test.runner.AndroidJUnitRunner'
    }
}
```

- **@Test:** This annotation is used to mark a method as a test case. The `@Test` annotation tells the Java compiler to run the method as a test case when the class is compiled.
- **@Before:** This annotation is used to mark a method as a setup method. The `@Before` annotation tells the Java compiler to run the method before each test case in the class. This is useful for setting up the test environment before each test case is run.
- **@After:** This annotation is used to mark a method as a tear-down method. The `@After` annotation tells the Java compiler to run the method after each test case in the class. This is useful for cleaning up the test environment after each test case is run.
- **@Ignore:** This annotation is used to mark a method as an ignored test case. The `@Ignore` annotation tells the Java compiler to not run the method as a test case. This is useful for temporarily ignoring a test case that you are not yet ready to run.

An **assertion** is the core of your test. It’s a code statement that checks that your code or app behaved as expected. In this case, the assertion is `assertEquals(4, 2 + 2)` which checks that 4 is equal to 2 + 2.

## **Given, When, Then**

One way to think about the structure of a test is to follow the Given, When, Then testing mnemonic. This divides your test into three parts:

- Given: Setup the objects and the state of the app that you need for your test. For this test, what is “given”.
- When: Do the actual action on the object you’re testing.
- Then: This is where you actually check what happens when you do the action where you check if the test passed or failed. This is usually a number of assert function calls.

Note that the “**Arrange, Act, Assert” (AAA)** testing mnemonic is a similar concept.

```kt
   fun getUserProfile_givenUserId_returnUser() {
        // GIVEN
        val userId = 1
        val user = User(userId,"","TestUser")
        // WHEN
        doReturn(Observable.just(user)).`when`(webservice).getMyProfile(userId)
        presenter.getUserProfile(anyString())
        // THEN
        verify(profileView).render(ProfileState.DataState(user))

    }
```



# Test Doubles

The solution to this is that when you’re testing the repository, *don’t use the real networking or database code*, but to instead use a test double. A test double is a version of a class crafted specifically for testing. It is meant to replace the real version of a class in tests. It’s similar to how a stunt double is an actor who specializes in stunts, and replaces the real actor for dangerous actions.



Sure! When you're testing your code, especially something like a repository that interacts with the network or a database, you don't want to actually use the real networking or database components. Instead, you create what's called a "test double."

Think of it like this: just as a stunt double replaces the real actor for dangerous scenes in a movie, a test double replaces the real class in your tests. It's a version of the class that's specifically designed for testing purposes. So, when you're testing your repository, you'll use this test double instead of the actual networking or database code. This helps isolate your tests and ensures they only focus on the specific functionality you're testing, without worrying about the complexities or potential issues of the real components.



Here are some types of test doubles:

## **Fake**

A test double that has a “working” implementation of the class, but it’s implemented in a way that makes it good for tests but unsuitable for production.

## **Mock**

A test double that tracks which of its methods were called. It then passes or fails a test depending on whether it’s methods were called correctly. When you mock an object it creates an empty implementation of that class.



# Naming Convention

The name of the test should help you understand what the test does. The naming convention should look like this:

`subjectUnderTest_actionOrInput_resultState`

- Subject under test is the method or class that is being tested (`getUserProfile`).
- Next is the action or input (`givenUserId`).
- Finally you have the expected result (`returnUser`).

----

### What is Unit Testing?

Unit testing is a software testing technique used to verify the correctness of individual units or components of a software system. A unit refers to the smallest testable part of an application, such as a method, function, or class. The purpose of unit testing is to isolate each unit of code and test it in isolation to ensure that it behaves as expected.



**The main objectives of unit testing are**:

- **Validation**: Aims to validate that each unit of code performs as intended. It checks whether the unit produces the expected output given a specific input.
- **Identification of defects**: Helps identify defects or bugs in the individual units of code. By testing each unit in isolation, developers can quickly pinpoint any errors and fix them before they impact other parts of the system.
- **Code quality**: Promotes good coding practices and code quality. It encourages developers to write modular, reusable, and maintainable code, as units are easier to test when they have clear boundaries and well-defined inputs and outputs.
- **Regression testing**: Act as a safety net when making changes to the codebase. By running unit tests after modifications, developers can ensure that the existing functionality remains intact and unaffected by the changes.



Unit testing follows the principles of the “Arrange, Act, Assert” (AAA) pattern. In this pattern, developers set up the necessary preconditions (Arrange), perform the action being tested (Act), and finally, verify the expected behavior or outcome (Assert).





### Common Challenges in Unit Testing For Android

While unit testing in Android offers numerous benefits, developers may encounter certain challenges during the process. Here are some common challenges in Android unit testing:

- **Dependency management**: Android apps often depend on external libraries, system components, or APIs. Managing these dependencies and ensuring they are properly mocked or stubbed during unit testing can be challenging. Dependency injection frameworks like Dagger can help alleviate this challenge by providing a way to manage and mock dependencies effectively.
- **Android-specific components**: Android applications rely on various Android-specific components such as Context, Intent, and SharedPreferences. These components are tightly coupled with the Android framework and can be difficult to mock or simulate in unit tests. Specialized libraries like Robolectric or Mockito-Android can assist in creating mock objects or shadowing Android components for testing purposes.
- **Asynchronous operations**: Android applications often involve asynchronous operations like network requests, database queries, or interactions with the user interface. Testing these asynchronous operations can be tricky as traditional unit tests are typically synchronous. 

Tools like Mockito’s **when().thenReturn()** and **verify()** can help simulate asynchronous behavior and ensure proper testing of these operations.



- **Legacy code and tight coupling**: Legacy codebases or codes with high coupling can present challenges in unit testing. Tight coupling makes it difficult to isolate individual units for testing, as changes in one unit may affect others. Refactoring and introducing dependency injection can help mitigate this challenge and make code more testable.
- **Resource access and configuration**: Android applications often rely on resources like strings, dimensions, or drawable. Accessing these resources directly in unit tests can be problematic, as they require a proper Android context. Libraries like AndroidX Test can provide utilities to create a test-specific application context and access resources during unit testing.
- **Time-consuming test execution**: As the size and complexity of an Android application grow, the number of unit tests can also increase significantly. Running a large number of tests can consume substantial time and resources, affecting the development workflow. Leveraging tools like Gradle’s build caching and running tests in parallel can help optimize test execution time.
- **Test maintenance**: Maintaining unit tests can be challenging, especially when the application undergoes frequent changes or refactorings. Tests may need to be updated or modified to reflect the changes in the codebase. Regular refactoring and ensuring tests have clear and concise assertions can make test maintenance more manageable.



### Best Practices For Unit Testing in Android

Unit testing in Android is essential for ensuring the quality and reliability of your codebase. Here are some best practices to follow when performing unit testing in Android:

1. **Write testable code**: Design your code with testability in mind. Aim for modular and loosely coupled components that can be easily isolated and tested independently. Avoid tight coupling, excessive dependencies, and static methods, as they make unit testing challenging.
2. **Focus on small units**: Unit tests should target small units of code, such as individual methods or classes, in isolation. This helps in maintaining test granularity and making tests more focused, readable, and maintainable.
3. **Use the AAA pattern**: Follow the Arrange-Act-Assert (AAA) pattern in your test methods. Set up the necessary preconditions and inputs (Arrange), invoke the method being tested (Act), and then verify the expected outcomes or behavior (Assert). This pattern improves the readability and structure of your tests.
4. **Mock dependencies**: Isolate the unit under test by mocking its dependencies. Use mocking frameworks like Mockito to create mock objects that simulate the behavior of dependencies. This allows you to test specific units of code without relying on the actual implementation details of the dependencies.
5. **Test boundary conditions and edge cases**: Ensure that your tests cover a wide range of scenarios, including boundary conditions and edge cases. Test both valid and invalid inputs, as well as different code paths and exceptional situations. This helps uncover potential bugs and ensures the robustness of your code.
6. **Test for exceptions**: Validate how your code handles exceptions and error conditions. Write tests to ensure that the appropriate exceptions are thrown when expected and that error-handling mechanisms are functioning correctly.
7. **Maintain test independence**: Each unit test should be independent and not rely on the state or outcome of other tests. Avoid sharing state between tests, as it can lead to test order dependencies and false positives or negatives. Isolate each test case and ensure it can run in isolation.
8. **Use parameterized tests**: Parameterized tests allow you to run the same test logic with different inputs and expected outputs. Use parameterized tests to minimize code duplication and cover multiple scenarios with a single test method.
9. **Regularly update and maintain tests**: Keep your tests up to date with code changes and refactorings. Regularly review and maintain your tests to ensure they remain accurate and reflect the current behavior of your codebase. Refactor tests along with the production code to keep them clean, readable, and maintainable.
10. **Integrate tests into the build process**: Incorporate unit tests into your build process to ensure they are executed automatically as part of your CI/CD pipeline or before code deployment. This helps catch issues early and ensures that tests are run consistently.







Links:

https://medium.com/@asadleo1995/unit-testing-for-android-a-beginners-guide-f8681cba3c22
