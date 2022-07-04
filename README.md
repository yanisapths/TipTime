# :money_with_wings: TipTime :money_with_wings: 
## Final Look

   ![image](https://user-images.githubusercontent.com/72002605/177097339-6a0f0058-6fe2-4edc-ac25-124a04f0da0e.png)


## XML Layouts
![image](https://user-images.githubusercontent.com/72002605/177024866-9e9ac247-1346-4604-b078-f63a82fce3e5.png)

## View binding

For convenience, Android also provides a feature called view binding. With a little more work up front, view binding makes it much easier and faster to call methods on the views in your UI. You'll need to enable view binding for your app in Gradle, and make some code changes.

##### Enable view binding

  - Open the app's build.gradle file ( Gradle Scripts > build.gradle (Module: Tip_Time.app) )
  - In the android section, add the following lines:

        buildFeatures {
            viewBinding = true
        }
        
  
  Note the message Gradle files have changed since last project sync.
  Press Sync Now.

  After a few moments, you should see a message at the bottom of the Android Studio window, Gradle sync finished. You can close the build.gradle file if you want.
  
  //previous MainActivity.kt
  
      class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
        }
      }
  
  //new MainActivity.kt
  
    import com.example.tiptime.databinding.ActivityMainBinding

      class MainActivity : AppCompatActivity() {

          lateinit var binding: ActivityMainBinding

          override fun onCreate(savedInstanceState: Bundle?) {
              super.onCreate(savedInstanceState)
              binding = ActivityMainBinding.inflate(layoutInflater)
              setContentView(binding.root)
          }
      }
      
Now when you need a reference to a View in your app, you can get it from the binding object instead of calling findViewById().

      // Old way with findViewById()
      val myButton: Button = findViewById(R.id.my_button)
      myButton.text = "A button"

      // Better way with view binding
      val myButton: Button = binding.myButton
      myButton.text = "A button"

      // Best way with view binding and no extra variable
      binding.myButton.text = "A button"
 
 
 ## Calculate the tip
  
  - Add click listener to the button , inside onCreate()
  
        binding.calculateButton.setOnClickListener{ calculateTip() } 
   
 - Still inside MainActivity class but outside onCreate(), add a helper method called calculateTip().
 
        fun calculateTp() {
          ...
        }
  
   toDouble() 
            needs to be called on a String. It turns out that the text attribute of an EditText is an Editable, because it represents text that can be changed. 
            Fortunately, you can convert an Editable to a String by calling toString() on it.

              val stringInTextField = binding.costOfService.text.toString()
              val cost = stringInTextField.toDouble()

   Get the tip percentage

                val tipPercentage = when(binding.tipOptions.checkedRadioButtonId) {
                    R.id.amazing_service -> 0.20
                    R.id.good_service -> 0.18
                    else -> 0.15
                }
                var tip = tipPercentage * cost

   Calculate the tip and round it up


              val roundUp = binding.roundupTip.isChecked
                if (roundUp) {
                    tip = kotlin.math.ceil(tip)
                }

                val formattedTip = NumberFormat.getCurrencyInstance().format(tip)
                binding.tipAmount.text = getString(R.string.tip_amount, formattedTip)

   Format the tip
            In strings.xml

              <string name="tip_amount">Tip Amount: %s</string>

   Open activity_main.xml (app > res > layout > activity_main.xml).
          - Find the tip_amount TextView.
          - Remove the line with the android:text attribute.

                 tools:text="Tip Amount: $10"

   ![image](https://user-images.githubusercontent.com/72002605/177025950-af6bd01a-dbfd-4daf-b731-189482941880.png)
   
   
Test & Debug
   
 Calling toDouble() on a string that is empty or a string that doesn't represent a valid decimal number doesn't work. 
    Fortunately Kotlin also provides a method called toDoubleOrNull() which handles these problems. 

   - Null is a special value that means "no value". 
    It's different from a Double having a value of 0.0 or an empty String with zero characters, "". 
   - Null means there is no value, no Double or no String. Many methods expect a value and may not know how to handle null and will stop
   
          val cost = stringInTextField.toDoubleOrNull()
              if (cost == null) {
                  binding.tipAmount.text = ""
                  return
              }
 
 Private 
   The method or variable is only visible to code within that class, in this case, MainActivity class. There's no reason for code outside MainActivity to call calculateTip(), so you can safely make it private.
   - Choose Make ‘calculateTip' ‘private', or add the private keyword or  _Alt+Shift+Enter_
   
   
## Change App Theme

- https://material.io/resources/color/#!/?view.left=0&view.right=0&primary.color=90CAF9&secondary.color=5C6BC0

Light Theme
![image](https://user-images.githubusercontent.com/72002605/177027849-6dc38181-c5c3-4dd0-8902-f74930e4642d.png)

Dark theme
![image](https://user-images.githubusercontent.com/72002605/177027877-8868e01b-3218-4d4e-9bf0-cefda6aeff1d.png)

