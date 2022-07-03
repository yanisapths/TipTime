# TipTime
:money_with_wings: :money_with_wings:

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

