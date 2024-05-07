###### When only onDestroy is called for an activity without onPause() and onStop()?

Answer :
When you calling finish() in onCreate function only onDestory is calling.
Consider you doing some opration in onCreate function after all you calling finish()
```kt
class MainActivity():AppCompatActivity(){

override fun onCreate(){
  setContentView()

 // some code
//some opration 
finish()    
}
  }
```
