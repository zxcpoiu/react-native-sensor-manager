react-native-sensor-manager
============================

Wrapper for react-native. Accelerometer, Gyroscope, Magnetometer, Step Counter, Thermometer are supported for now.

Add it to your project
-------------------------

`$ npm i react-native-sensor-manager --save`

### Option: With [`rnpm`](https://github.com/rnpm/rnpm)

`rnpm link`

### Option: Manually

Make alterations to the following files:

* `android/settings.gradle`

```gradle
...
include ':react-native-sensor-manager'
project(':react-native-sensor-manager').projectDir = new File(settingsDir, '../node_modules/react-native-sensor-manager/android')
```

* `android/app/build.gradle`

```gradle
...
dependencies {
    ...
    compile project(':react-native-sensor-manager')
}
```

* register module (in MainActivity.java)
 
  * For react-native below 0.19.0 (use `cat ./node_modules/react-native/package.json | grep version`)

```java
import com.sensormanager.SensorManagerPackage; // <------ add package

public class MainActivity extends Activity implements DefaultHardwareBackBtnHandler {

  ......

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mReactRootView = new ReactRootView(this);

    mReactInstanceManager = ReactInstanceManager.builder()
      .setApplication(getApplication())
      .setBundleAssetName("index.android.bundle")
      .setJSMainModuleName("index.android")
      .addPackage(new MainReactPackage())
      .addPackage(new SensorManagerPackage())      // <------- add package
      .setUseDeveloperSupport(BuildConfig.DEBUG)
      .setInitialLifecycleState(LifecycleState.RESUMED)
      .build();

    mReactRootView.startReactApplication(mReactInstanceManager, "ExampleRN", null);

    setContentView(mReactRootView);
  }

  ......

}
```

  * For react-native 0.19.0 and higher
```java
import com.sensormanager.SensorManagerPackage; // <------ add package

public class MainActivity extends ReactActivity {
   // ...
    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
        new MainReactPackage(), // <---- add comma
        new SensorManagerPackage() // <---------- add package
      );
    }
```

Api
----

### Setup
```js
import React, {
  DeviceEventEmitter // will emit events that you can listen to
} from 'react-native';

var mSensorManager = require('NativeModules').SensorManager;
```


### Accelerometer
```js
mSensorManager.startAccelerometer(100); // To start the accelerometer with a minimum delay of 100ms between events.
DeviceEventEmitter.addListener('Accelerometer', function (data) {
  /**
  * data.x
  * data.y
  * data.z
  **/
});
mSensorManager.stopAccelerometer();
```

### Gyroscope
```js
DeviceEventEmitter.addListener('Gyroscope', function (data) {
  /**
  * data.x
  * data.y
  * data.z
  **/
});
mSensorManager.startGyroscope(100);
mSensorManager.stopGyroscope();
```

### Magnetometer
```js
mSensorManager.startMagnetometer(100);
DeviceEventEmitter.addListener('Magnetometer', function (data) {
  /**
  * data.x
  * data.y
  * data.z
  **/
});
mSensorManager.stopMagnetometer();
```

### Step Counter
```js
mSensorManager.startStepCounter(1000);
DeviceEventEmitter.addListener('StepCounter', function (data) {
  /**
  * data.steps
  **/
});
mSensorManager.stopStepCounter();
```

### Thermometer
```js
mSensorManager.startThermometer(1000);
DeviceEventEmitter.addListener('Thermometer', function (data) {
  /**
  * data.temp
  **/
});
mSensorManager.stopThermometer();
```
