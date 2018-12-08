# Title:
CheckedTextView

# Introduction:
In android programming, a CheckedTextView may be refered as an extension of a normal TextView which supports the 'checkable' interface and displays. It can be used in many cases such as in a ListView where a choice is checked/selected, the following function changes to setChoiceMade from CHOICE_MODE_NONE. If there is a situation where a user gets to select only one choice, the function CHOICE_MODE_SINGLE is set. Otherwise if more than one choice/unlimited checked, then the function CHOICE_MODE_MULTIPLE is set.

# History:
CheckedTextView was introduced in API 1, version_code "BASE". The package library is in android.widget <br/>
There are many more functions associated with CheckedTextview, we are going to go through the one I used in my example project below <br/>

# Major methods/attributes:
public class CheckedTextView extends TextView implements Checkable <br/>
The following is in the XML atributes: <br/>
android:checkMark - to provide a drawble or graphic checkMark. <br/>
android:checkMarkTint - This is the tint when applying to check mark. <br/>
android:checkMarkTintMode - This is used to apply a check mark tint in blending mode <br/>
android:checked - This is an 'either or' which indicates the checked state. It can be a boolean value such as true or false. <br/>
The 4 attributes are inherited from the class android.widget.TextView and android.view.View <br/>



# Example Project:
The check mark used: ![checkmark](https://user-images.githubusercontent.com/42982622/49680495-e1fd1a00-fa62-11e8-8bf6-ca75cfb8a137.png) <br/>
![checkedtextview example](https://user-images.githubusercontent.com/42982622/49680338-5c2c9f00-fa61-11e8-81a3-cf94315e3cf2.PNG) <br/>
A summary on how to create a CheckedTextView: <br/>
1. Create a listView in activity_main.xml <br/>
2. Create a CheckedTextView in another newly created list_view_items.xml <br/>
3. Create a custom adapter "public class CustomAdapter extends BaseAdapter" in CustomAdapter.java <br/>
4. Create a variable array to hold the strings in MainActivity.java <br/>
5. Declare & findViewById of listview in MainActivity.java <br/>
6. Parse the strings to CustomAdapter and set the adapter according to the listView in MainActivity.java <br/>
7. Create an array variable to hold the parsed Strings in CustomAdapter.java <br/>
8. Get the view from list_view_items, CheckedTextView and set the text to it's position. <br/>
9. Finally, we create a listener when user interacts with the selection he/she makes. <br/>

# Code:
This is a link to the complete project on Git-hub: https://github.com/Kritish007/assignment3 <br/>
### ```MainActivity.java```

```
package com.example.kriti.assignment3;
import android.graphics.Color;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.CheckedTextView;
import android.widget.ListView;

public class MainActivity extends AppCompatActivity {
    //String value;
    String[] settingsList = {"Enable power saving", "Enable fail-safe",
                             "Auto power off your device", "Enable offline-mode",
                             "Enable stable capture"}; // Names of the settings
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ListView listView = (ListView) findViewById(R.id.listView); // Brings up the listView
        CustomAdapter customAdapter = new CustomAdapter(getApplicationContext(), settingsList); //settingsList is parsed to CustomAdapter
        listView.setAdapter(customAdapter); //Set adapter according to the listView
    }
}
```
### ```list_view_items.xml```
 ```
 <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <CheckedTextView
        android:id="@+id/CheckedTextView"
        android:layout_width="fill_parent"
        android:layout_height="50dp"
        android:checked="false"
        android:gravity="center_vertical"
        android:paddingLeft="15dp"
        android:textColor="#000"
        android:text="CheckedTextView" />
    <!-- CheckedTextView In Android -->
</LinearLayout>
```
### ```CustomAdapter.java```
 ```
 package com.example.kriti.assignment3;
import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.BaseAdapter;
import android.widget.CheckedTextView;

public class CustomAdapter extends BaseAdapter{

    String[] settingName; // Creating a variable array to store the names
    Context context; // Adapter context when user reacts
    LayoutInflater textInflater; // Inflates the adapter as per the text entered

    public CustomAdapter(Context context, String[] settingName) {
        this.context = context;
        this.settingName = settingName;
        textInflater = (LayoutInflater.from(context));

    }

    @Override
    public int getCount() {
        return settingName.length; //length in bytes
    }

    @Override
    public Object getItem(int position) {
        return null;
    }

    @Override
    public long getItemId(int position) {
        return 0;
    }

    @Override
    public View getView(int position, View view, ViewGroup parent) {
        view = textInflater.inflate(R.layout.list_view_items, null);//Brings up the view to the screen
        final CheckedTextView CheckedTextViewAll = (CheckedTextView) view.findViewById(R.id.CheckedTextView); //Get ViewById
        CheckedTextViewAll.setText(settingName[position]); //setText to it's position

 /*The button listener might seem the opposite but this is how it works:
It first checks if a selection/checked is already there when the user selects a choice
If selection is checked, do nothing
Else display the check-mark. */

        CheckedTextViewAll.setOnClickListener(new View.OnClickListener() {
            @Override
    public void onClick(View v) {
                if (CheckedTextViewAll.isChecked()) { // to determine if already checked
                    CheckedTextViewAll.setCheckMarkDrawable(0); // if checked, set drawable to 0/do nothing
                    CheckedTextViewAll.setChecked(false); // set checked to false/do nothing
                } else { //else if selection is not checked then display check
                    CheckedTextViewAll.setCheckMarkDrawable(R.drawable.checkmark); // display checked image
                    CheckedTextViewAll.setChecked(true); // set to checked
                }
            }
        }); // End of listener
        return view; // return view
    }
}
```
### ```activity_main.xml```
 ```
 <?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ListView
        android:id="@+id/listView"
        android:layout_width="385dp"
        android:layout_height="398dp"
        android:layout_marginTop="8dp"
        android:layout_marginBottom="8dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:layout_editor_absoluteX="2dp" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="381dp"
        android:layout_height="57dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginBottom="8dp"
        android:text="@string/your_device_s_settings"
        android:textColor="@android:color/holo_red_light"
        android:textSize="27sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.018" />

</android.support.constraint.ConstraintLayout>
 ```

# Reference:

https://developer.android.com/reference/android/widget/CheckedTextView <br/>
https://abhiandroid.com/ui/checkedtextview <br/>
https://c1ctech.com/android-checkedtextview-example/ <br/>
