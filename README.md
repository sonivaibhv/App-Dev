# App-Dev
In this repository you will learn how to make a simple phone call android application

        ANDROID APPLICATION DEVELOPMENT
STEP 1 : Create Android Project 
          Name of project = phone
STEP 2 : Give Package Name
          Name of package is = phone.project
STEP 3 : Now Click finish
STEP 4 : Open main.xml file under layout folder and copy above code
          
          <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

   

    <Button
        android:id="@+id/buttonCall"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Domanic Toratto" />
  </LinearLayout>
        
STEP 5 : Open phoneActivity.java file and copy above code

          package phone.project;
import android.app.Activity;


import android.content.Context;
import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.telephony.PhoneStateListener;
import android.telephony.TelephonyManager;
import android.util.Log;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;

public class PhoneActivity extends Activity {
	final Context context=this;
	private Button button;
    /** Called when the activity is first created. */
    @Override
    public void onCreate(Bundle savedInstanceState) {
    	super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        button=(Button)findViewById(R.id.buttonCall);
        //add phoneStateListener;
        PhoneCallListener phoneListener=new PhoneCallListener();
        TelephonyManager telephonyManager=(TelephonyManager)this.getSystemService(Context.TELEPHONY_SERVICE);
        telephonyManager.listen(phoneListener,PhoneStateListener.LISTEN_CALL_STATE);

    
        //add button listener
        button.setOnClickListener(new OnClickListener(){
        	 public void onClick(View arg0){
        		 Intent callIntent=new Intent(Intent.ACTION_CALL);
        		 callIntent.setData(Uri.parse("tel:123456789"));   		
        		 startActivity(callIntent);
        		 
        	 }
        });
    }
//monitor phone call activities
    private class PhoneCallListener extends PhoneStateListener{
    	private boolean isPhoneCalling=false;
    	String LOG_TAG="LOGGING 123";
    	@Override
public void onCallStateChanged(int state,String incomingNumber){
    		if(TelephonyManager.CALL_STATE_RINGING==state){
    			//phone ringing
    			Log.i(LOG_TAG,"RINGING,number:"+incomingNumber);
    		}
    		if(TelephonyManager.CALL_STATE_OFFHOOK==state){
    			//active
    			Log.i(LOG_TAG,"OFFHOOK");
    			isPhoneCalling=true;
    		}
    		if(TelephonyManager.CALL_STATE_IDLE==state){
    			//run when class initial and phone call ended,
    			//need detect flag from CALL_STATE_OFFHOOK
    			Log.i(LOG_TAG,"IDLE");
    			if(isPhoneCalling){
    				Log.i(LOG_TAG,"restart app");
    				//restart app
    				Intent i=getBaseContext().getPackageManager().getLaunchIntentForPackage(
    			getBaseContext().getPackageName());
    				i.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
    				startActivity(i);
    				isPhoneCalling=false;
    			}
    		}
    			 
    	}
    }
}


STEP 6 : Now open AndroidManifest.xml file and copy above code

          <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="phone.project"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk android:minSdkVersion="8" />
<uses-permission android:name="android.permission.CALL_PHONE"	/>
<uses-permission android:name="android.permission.READ_PHONE_STATE"	/>
    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <activity
            android:name=".PhoneActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>

STEP 7 : now run your project .


    

