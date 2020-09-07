---
title: Prerequisites and Configurations for DL4J in Android
short_title: Android Prerequisites
description: Setting up and configuring Android Studio for DL4J.
category: Mobile
weight: 1
---

# Setup

While neural networks are typically run on powerful computers using multiple GPUs, the compatibility of Deeplearning4J with the Android platform makes using DL4J neural networks in android applications a possibility. This tutorial will cover the basics of setting up android studio for building DL4J applications. Several configurations for dependencies, memory management, and compilation exclusions needed to mitigate the limitations of low powered mobile device are outlined below. If you just want to get a DL4J app running on your device, you can jump ahead to a simple demo application which trains a neural network for Iris flower classification available [here](linear-classifier.md#iris-classifier-demo).

## Prerequisites

* Android Studio 3.6.3 or newer, which can be downloaded [here](https://developer.android.com/studio/index.html#Other). 
* Android Studio version 3.6.3 and higher comes with the latest OpenJDK embedded; however, it is recommended to have the JDK installed on your own as you are then able to update it independent of Android Studio. Android Studio 3.0 and later supports all of Java 7 and a subset of Java 8 language features. Java JDKs can be downloaded from the Oracle or OpenJDK website.
* Within Android studio, the Android SDK Manager can be used to install Android Build tools 24.0.1 or later, SDK platform 24 or later, and the Android Support Repository. 
* An Android device or an emulator running API level 21 or higher. A minimum of 200 MB of internal storage space free is recommended.

It is also recommended that you download and install IntelliJ IDEA, Maven, and the complete dl4j-examples directory for building and building and training neural nets on your desktop instead of android studio. With the setup we are giving you here you can use dl4j on your android device and emulator. You need to add the regular dependencies to use dl4j in the `\app\src\test\java\` tests. 

## Required Dependencies

In order to use Deeplearning4J in your Android projects, you will need to add the following dependencies to your app module’s build.gradle file. Depending on the type of neural network used in your application, you may need to add additional dependencies.

```groovy
implementation (group: 'org.deeplearning4j', name: 'deeplearning4j-core', version: '{{page.version}}') {
    exclude group: 'org.bytedeco', module: 'opencv-platform'
    exclude group: 'org.bytedeco', module: 'leptonica-platform'
    exclude group: 'org.bytedeco', module: 'hdf5-platform'
}
implementation group: 'org.nd4j', name: 'nd4j-native', version: '1.0.0-beta7'
implementation group: 'org.nd4j', name: 'nd4j-native', version: '1.0.0-beta7', classifier: "android-arm"
implementation group: 'org.nd4j', name: 'nd4j-native', version: '1.0.0-beta7', classifier: "android-arm64"
implementation group: 'org.nd4j', name: 'nd4j-native', version: '1.0.0-beta7', classifier: "android-x86"
implementation group: 'org.nd4j', name: 'nd4j-native', version: '1.0.0-beta7', classifier: "android-x86_64"
implementation group: 'org.bytedeco', name: 'openblas', version: '0.3.7-1.5.2'
implementation group: 'org.bytedeco', name: 'openblas', version: '0.3.7-1.5.2', classifier: "android-arm"
implementation group: 'org.bytedeco', name: 'openblas', version: '0.3.7-1.5.2', classifier: "android-arm64"
implementation group: 'org.bytedeco', name: 'openblas', version: '0.3.7-1.5.2', classifier: "android-x86"
implementation group: 'org.bytedeco', name: 'openblas', version: '0.3.7-1.5.2', classifier: "android-x86_64"
implementation group: 'org.bytedeco', name: 'opencv', version: '4.1.2-1.5.2'
implementation group: 'org.bytedeco', name: 'opencv', version: '4.1.2-1.5.2', classifier: "android-arm"
implementation group: 'org.bytedeco', name: 'opencv', version: '4.1.2-1.5.2', classifier: "android-arm64"
implementation group: 'org.bytedeco', name: 'opencv', version: '4.1.2-1.5.2', classifier: "android-x86"
implementation group: 'org.bytedeco', name: 'opencv', version: '4.1.2-1.5.2', classifier: "android-x86_64"
implementation group: 'org.bytedeco', name: 'leptonica', version: '1.78.0-1.5.2'
implementation group: 'org.bytedeco', name: 'leptonica', version: '1.78.0-1.5.2', classifier: "android-arm"
implementation group: 'org.bytedeco', name: 'leptonica', version: '1.78.0-1.5.2', classifier: "android-arm64"
implementation group: 'org.bytedeco', name: 'leptonica', version: '1.78.0-1.5.2', classifier: "android-x86"
implementation group: 'org.bytedeco', name: 'leptonica', version: '1.78.0-1.5.2', classifier: "android-x86_64"
testimplementation 'junit:junit:4.12'
```

You may also need to add the following compile options:

```groovy
compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
```

DL4J depends on ND4J, which is a library that offers fast n-dimensional arrays. ND4J in turn depends on a platform-specific native code library called JavaCPP, therefore you must load a version of ND4J that matches the architecture of the Android device. Both -x86 and -arm types can be included to support multiple device processor types.

After adding the above to the build.gradle file, try syncing Gradle with to see if any exclusions are needed. The error message will identify the file path that should be added to the list of exclusions. An example error message with file path is: _&gt; More than one file was found with OS independent path 'org/bytedeco/javacpp/ windows-x86\_64/msvp120.dll'_ 

A conflict in the junit module versions often causes the following error: _&gt; Conflict with dependency 'junit:junit' in project ':app'. Resolved versions for app \(4.8.2\) and test app \(4.12\) differ_. This can be suppressed by forcing all of the junit modules to use the same version with the following:

```java
configurations.all {
    resolutionStrategy.force 'junit:junit:4.12'
}
```

## Managing Dependencies with ProGuard

The DL4J dependencies compile a large number of files. ProGuard can be used to minimize your APK file size. ProGuard detects and removes unused classes, fields, methods, and attributes from your packaged app, including those from code libraries. You can learn more about using Proguard [here](https://developer.android.com/studio/build/shrink-code.html). To enable code shrinking with ProGuard, add minifyEnabled true to the appropriate build type in your build.gradle file.

```java
buildTypes {
    release {
        minifyEnabled true
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
    }
}
```

It is recommended to upgrade your ProGuard in the Android SDK to the latest release \(5.1 or higher\). Note that upgrading the build tools or other aspects of your SDK might cause Proguard to reset to the version shipped with the SDK. In order to force ProGuard to use a version of other than the Android Gradle default, you can include this in the buildscript of `build.gradle` file:

```java
buildscript {
    configurations.all {
        resolutionStrategy {
            force 'net.sf.proguard:proguard-gradle:5.3.2'
        }
    }
}
```

Testing your app is the best way to check if any errors are being caused by inappropriately removed code; however, you can also inspect what was removed by reviewing the usage.txt output file saved in /build/outputs/mapping/release/.

To fix errors and force ProGuard to retain certain code, add a -keep line in the ProGuard configuration file. For example:

```java
-keep public class MyClass
```

## Memory Management

It may also be advantageous to increase the allocated memory to your app by adding android:largeHeap="true" to the manifest file. Allocating a larger heap means that you decrease the risk of throwing an OutOfMemoryError during memory intensive operations.

```markup
android:largeHeap="true"
```

ND4J offers an additional memory-management model: workspaces. Workspaces allow you to reuse memory for cyclic workloads without the JVM Garbage Collector for off-heap memory tracking. D4j Workspace allows for memory to be preallocated before a try / catch block and reused over in over within that block.

If your training process uses workspaces, it is recommended that you disable or reduce the frequency of periodic GC calls prior to your model.fit\(\) call.

```java
// this will limit frequency of gc calls to 5000 milliseconds
Nd4j.getMemoryManager().setAutoGcWindow(5000)

// this will totally disable it
Nd4j.getMemoryManager().togglePeriodicGc(false);
```

The example below illustrates the use of a Workspace for memory allocation.  More information concerning ND4J Workspaces can be found [here](../config/config-memory/config-workspaces.md).

```java
import org.nd4j.linalg.api.memory.MemoryWorkspace;
import org.nd4j.linalg.api.memory.conf.WorkspaceConfiguration;
import org.nd4j.linalg.api.memory.enums.AllocationPolicy;
import org.nd4j.linalg.api.memory.enums.LearningPolicy;


//Make sure to run on a background thread, this is where we will initiate the Workspace
protected INDArray doInBackground(String... params) {

    // we will create configuration with 10MB memory space preallocated
    WorkspaceConfiguration initialConfig = WorkspaceConfiguration.builder()
        .initialSize(10 * 1024L * 1024L)
        .policyAllocation(AllocationPolicy.STRICT)
        .policyLearning(LearningPolicy.NONE)
        .build();

    INDArray result = null;

    try(MemoryWorkspace ws = Nd4j.getWorkspaceManager().getAndActivateWorkspace(initialConfig, "SOME_ID")) {
        // now, INDArrays created within this try block will be allocated from this workspace pool
    
        //Load a trained model
        File file = new File(Environment.getExternalStorageDirectory() + "/trained_model.zip");
        MultiLayerNetwork restored = ModelSerializer.restoreMultiLayerNetwork(file);
    
        // Create input in INDArray
        INDArray inputData = Nd4j.zeros(1, 4);
    
        inputData.putScalar(new int[]{0, 0}, 1);
        inputData.putScalar(new int[]{0, 1}, 0);
        inputData.putScalar(new int[]{0, 2}, 1);
        inputData.putScalar(new int[]{0, 3}, 0);
    
        result = restored.output(inputData);
    
    }
    catch(IOException ex){Log.d("AsyncTaskRunner2 ", "catchIOException = " + ex  );}

    return result;
}
```

## Saving and Loading Networks on Android

Practical considerations regarding performance limits are needed when building Android applications that run neural networks. Training a neural network on a device is possible, but should only be attempted with networks with limited numbers of layers, nodes, and iterations. The first Demo app [DL4JIrisClassifierDemo](linear-classifier.md#dl-4-jirisclassifierdemo) is able to train on a standard device in about 15 seconds.

When training on a device is a reasonable option, the application performance can be improved by saving the trained model on external storage once an initial training is complete. The trained model can then be used as an application resource. This approach is useful for training networks with data obtained from user input. The following code illustrates how to train a network and save it on external resources.

For API 23 and greater, you will need to include the permissions in your manifest and also programmatically request the read and write permissions in your activity. The required Manifest permissions are:

```markup
<manifest>
        <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        ...
```

You need to implement ActivityCompat.OnRequestPermissionsResultCallback in the activity and then check for permission status.

```java
public class MainActivity extends AppCompatActivity
        implements ActivityCompat.OnRequestPermissionsResultCallback {

    private static final int REQUEST_EXTERNAL_STORAGE = 1;
    private static String[] PERMISSIONS_STORAGE = {
            Manifest.permission.READ_EXTERNAL_STORAGE,
            Manifest.permission.WRITE_EXTERNAL_STORAGE
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        verifyStoragePermission(MainActivity.this);
    //…
    }

    public static void verifyStoragePermission(Activity activity) {
        // Get permission status
        int permission = ActivityCompat.checkSelfPermission(activity, Manifest.permission.WRITE_EXTERNAL_STORAGE);
        if (permission != PackageManager.PERMISSION_GRANTED) {
        // We don't have permission we request it
        ActivityCompat.requestPermissions(
                    activity,
                    PERMISSIONS_STORAGE,
                    REQUEST_EXTERNAL_STORAGE
            );
        }
    }
```

To save a network after training on the device use a OutputStream within a try catch block.

```java
try {
    File file = new File(Environment.getExternalStorageDirectory() + "/trained_model.zip");
    OutputStream outputStream = new FileOutputStream(file);
    boolean saveUpdater = true;
    ModelSerializer.writeModel(myNetwork, outputStream, saveUpdater);

} catch (Exception e) {
    Log.e("saveToExternalStorage error", e.getMessage());
}
```

To load the trained network from storage you can use the restoreMultiLayerNetwork method.

```java
try{
    //Load the model
    File file = new File(Environment.getExternalStorageDirectory() + "/trained_model.zip");
    MultiLayerNetwork restored = ModelSerializer.restoreMultiLayerNetwork(file);

} catch (Exception e) {
    Log.e("Load from External Storage error", e.getMessage());
}
```

For larger or more complex neural networks like Convolutional or Recurrent Neural Networks, training on the device is not a realistic option as long processing times during network training run the risk of generating an OutOfMemoryError and make for a poor user experience. As an alternative, the Neural Network can be trained on the desktop, saved via ModelSerializer, and then loaded as a pre-trained model in the application. Using a pre-trained model in you Android application can be achieved with the following steps:

* Train the yourModel on desktop and save via modelSerializer.
* Create a raw resource folder in the res directory of the application.
* Copy yourModel.zip file into the raw folder.
* Access it from your resources using an inputStream within a try / catch block.

```java
try {
// Load name of model file (yourModel.zip).
        InputStream is = getResources().openRawResource(R.raw.yourModel);

// Load yourModel.zip.
        MultiLayerNetwork restored = ModelSerializer.restoreMultiLayerNetwork(is);

// Use yourModel.
        INDArray results = restored.output(input)
        System.out.println("Results: "+ results );
// Handle the exception error
} catch(IOException e) {
        e.printStackTrace();
    }
```

## Snapshots

If you choose to use a SNAPSHOT version of the dependencies with gradle, you will need to create the a pom.xml file in the root directory and run `mvn -U compile` on it from the terminal. You will also need to include `mavenLocal()` in the `repository {}` block of the build.gradle file. An example pom.xml file is provided below.

```markup
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.deeplearning4j</groupId>
    <artifactId>snapshots</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <dependencies>
       <dependency>
            <groupId>org.nd4j</groupId>
            <artifactId>nd4j-native-platform</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.deeplearning4j</groupId>
            <artifactId>deeplearning4j-core</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
    <repositories>
        <repository>
            <id>sonatype-nexus-snapshots</id>
            <url>https://oss.sonatype.org/content/repositories/snapshots</url>
            <releases>
                <enabled>false</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
                <updatePolicy>always</updatePolicy>
            </snapshots>
        </repository>
    </repositories>
</project>
```



