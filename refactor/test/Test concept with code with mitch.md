
Local Unit Test :

JUnit5  -> new version but not complete. and some part of android cannot test in JUnit5 // check it maybe now you able to test them

Junit 4  -> 

Mockito -> popular library for create Mocks of objects. (Fake objects) like test network request so you need to mock network request. you can mock your repository class and you can define certain responses from server for example give in this condition return this response and give in this condition return another response.
you can mock the class and make ensure   your login work correctly. give certain input and you get certain output.

AndroidX -> 
we use it for testing Room Database , Room DAO , ApplicationProvider, InstantTaskExecutorRule: doing things on background because we can't do it by defult


TDD : Test-driven-Development
Local Unit Test : use JVM to run and very fast and don't need to run on emulator.


Testing on android:
- Local Unit Tests
- Instrumentation Tests
- UI Test

![[Screenshot (407).png]]




Unit:
Local computer
JVM 
Very fast

Juni5 and mockito


Instrumentation:
Similar to Local Unit tests
Test Specific functionality, functionality uses things like activity,fragment, context, services
all this kind of lifecycle stuff that strickly uniqe to android.
for example we test our class that have access to application, context, activity
and you need run Instrumentation on real android device emulator because they about android framework.

and use Junit 4 becasue of loboratlic


UI:
Simulate a person using your app
For example if user clickbutton very fast or very slow if something happend you can know with this test.
Run device or emulator 

 -----
 Testing in android Phliph 

TDD:
- Main principle: Write the test case before the implementation the function (only for unit tests)
-   -1  Write the function signature
-    2- write the test cases for that function


first think about the function signature and define the function signature but no content of that function yet. then second step is write the test case that should test the function and then testcase should always fail at first because we don't have any content in that function so the test case must fail of course and after that at the we finally implement the function in a way  that satisfies our  test cases.

what makes a good test?
1- Scope    how much of our actual code in our function that we want to test is covered by our single test case.
2- Speed     speed is  pretty self-explanatory so it just means how fast our test case runs
3- Fidelity     how close our test case in actually to a real scenario because that's what we want to verify in the end that our app works well in the real world.

what important is we want to prevent so-call flaky tests those are tests that sometimes succeed and sometimes fail.  like random between 1 to 5 and you wanna say when it == 2 return true
in some case it might fail and sometimes it might succssed.
always make sure that your tests are independent and also that your test are not dependent on another test case.

How many test cases should you write?


![[Screenshot (408).png]]

in TDD meaning of first create function signature is this:
```kotlin

    fun validationRegister(
        userName:String,
        password:String,
        confirmPassword:String
    ):Boolean {}
    
```

step two is we must think about it when our condition get false and when get true
![[Screenshot (409).png]]


because our validate fun return boolean we can access to isTrue or false 
assertThat(result).isFalse()

in other case like this 
 assertThat("result").contains()
because the assertThat is string


and after that we need the actual impelemanation base on our tests.
