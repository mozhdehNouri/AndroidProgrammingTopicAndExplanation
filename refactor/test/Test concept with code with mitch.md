
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

 
What is Exactly Unit testing:
Unit:
